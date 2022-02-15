# Get certificate from cluster

One way to give access to kubernetes cluster is with a certificate. You can also use external systems like Azure Active and AWS IAM.

It's best to use an external identity system so that we don't have to maintain two different sources of identity, but in some cases this isn’t possible and you need to use certificate.

First, you need to generate a certificate signing request to generate a new certificate. Here is a simple Go program to do this:

```go
package main

import (
	"crypto/rand"
	"crypto/rsa"
	"crypto/x509"
	"crypto/x509/pkix"
	"encoding/asn1"
	"encoding/pem"
	"os"
)

func main() {
	name := os.Args[1]
	user := os.Args[2]

	key, err := rsa.GenerateKey(rand.Reader, 1024)
	if err != nil {
		panic(err)
	}
	keyDer := x509.MarshalPKCS1PrivateKey(key)
	keyBlock := pem.Block{
		Type:  "RSA PRIVATE KEY",
		Bytes: keyDer,
	}
	keyFile, err := os.Create(name + "-key.pem")
	if err != nil {
		panic(err)
	}
	pem.Encode(keyFile, &keyBlock)
	keyFile.Close()

	commonName := user
	// You may want to update these too
	emailAddress := "someone@myco.com"

	org := "My Co, Inc."
	orgUnit := "Widget Farmers"
	city := "Seattle"
	state := "WA"
	country := "US"

	subject := pkix.Name{
		CommonName:         commonName,
		Country:            []string{country},
		Locality:           []string{city},
		Organization:       []string{org},
		OrganizationalUnit: []string{orgUnit},
		Province:           []string{state},
	}

	asn1, err := asn1.Marshal(subject.ToRDNSequence())
	if err != nil {
		panic(err)
	}
	csr := x509.CertificateRequest{
		RawSubject:         asn1,
		EmailAddresses:     []string{emailAddress},
		SignatureAlgorithm: x509.SHA256WithRSA,
	}

	bytes, err := x509.CreateCertificateRequest(rand.Reader, &csr, key)
	if err != nil {
		panic(err)
	}
	csrFile, err := os.Create(name + ".csr")
	if err != nil {
		panic(err)
	}

	pem.Encode(csrFile, &pem.Block{Type: "CERTIFICATE REQUEST", Bytes: bytes})
	csrFile.Close()
}
```

You can run this as `go run csr-gen.go client <user-name>`

This creates files called client-key.pem and client.csr. You then can run the following script to create and download a new certificate:

```bash
#!/bin/bash

csr_name="my-client-csr"
name="${1:-my-user}"

csr="${2}"


cat <<EOF | kubectl create -f -
apiVersion: certificates.k8s.io/v1beta1
kind: CertificateSigningRequest
metadata:
  name: ${csr_name}
spec:
  groups:
  - system:authenticated
  request: $(cat ${csr} | base64 | tr -d '\n')
  usages:
  - digital signature
  - key encipherment
  - client auth
EOF

echo
echo "Approving signing request."
kubectl certificate approve ${csr_name}

echo
echo "Downloading certificate."
kubectl get csr ${csr_name} -o jsonpath='{.status.certificate}' \
	| base64 --decode > $(basename ${csr} .csr).crt

echo
echo "Cleaning up"
kubectl delete csr ${csr_name}

echo
echo "Add the following to the 'users' list in your kubeconfig file:"
echo "- name: ${name}"
echo "  user:"
echo "    client-certificate: ${PWD}/$(basename ${csr} .csr).crt"
echo "    client-key: ${PWD}/$(basename ${csr} .csr)-key.pem"
echo
echo "Next you may want to add a role-binding for this user."
```

This script prints out the final information that you can add to a kubeconfig file to enable that user. Of course, the user has no access privileges, so you will need to apply Kubernetes RBAC for the user in order to grant them privileges to a namespace.

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: example
  namespace: my-namespace
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: edit
subjects:
- apiGroup: rbac.authorization.k8s.io
  kind: User
  name: myuser
```
