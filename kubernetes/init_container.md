# initContainers

initContainers run to completion before the main container of the pod runs. 

They are pretty useful! 

- Init containers can contain utilities or custom code for setup that are not present in an app image. For example, there is no need to make an image FROM another image just to use a tool like sed, awk, python, or dig during setup.
- The application image builder and deployer roles can work independently without the need to jointly build a single app image.
- Init containers can run with a different view of the filesystem than app containers in the same Pod. Consequently, they can be given access to Secrets that app containers cannot access.
- Because init containers run to completion before any app containers start, init containers offer a mechanism to block or delay app container startup until a set of preconditions are met. Once preconditions are met, all of the app containers in a Pod can start in parallel.
- Init containers can securely run utilities or custom code that would otherwise make an app container image less secure. By keeping unnecessary tools separate you can limit the attack surface of your app container image.

## Examples

```yaml
# This example defines a simple Pod that has two init containers. The first waits for myservice, and the second waits for mydb. Once both init containers complete, the Pod runs the app container from its spec section.
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
  labels:
    app: myapp
spec:
  containers:
  - name: myapp-container
    image: busybox:1.28
    command: ['sh', '-c', 'echo The app is running! && sleep 3600']
  initContainers:
  - name: init-myservice
    image: busybox:1.28
    command: ['sh', '-c', "until nslookup myservice.$(cat /var/run/secrets/kubernetes.io/serviceaccount/namespace).svc.cluster.local; do echo waiting for myservice; sleep 2; done"]
  - name: init-mydb
    image: busybox:1.28
    command: ['sh', '-c', "until nslookup mydb.$(cat /var/run/secrets/kubernetes.io/serviceaccount/namespace).svc.cluster.local; do echo waiting for mydb; sleep 2; done"]

---

# this example is more complete and shows how to get the correct creds and access
---
apiVersion: v1
kind: ServiceAccount
metadata:
  name: demo
  labels:
    app: demo
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: demo
  labels:
    app: demo
rules:
- apiGroups:
  - ""
  resources:
  - pods
  - namespaces
  verbs:
  - get
  - list
  - watch
---
kind: ClusterRoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: demo
roleRef:
  kind: ClusterRole
  name: demo
  apiGroup: rbac.authorization.k8s.io
subjects:
- kind: ServiceAccount
  name: demo
  namespace: default
---
apiVersion: v1
kind: Pod
metadata:
  labels:
    app: demo
spec:
  containers:
  - name: myapp-container
    image: busybox:1.28
    command: ['sh', '-c', 'echo The app is running! && sleep 3600']
  initContainers:
  - name: init-dependencies
    image: alpine:3.13.6
    args:
    - /bin/sh
    - -c
    - |
      apk update && apk add jq curl
      # Point to the internal API server hostname
      APISERVER=https://kubernetes.default.svc
      # Path to ServiceAccount token
      SERVICEACCOUNT=/var/run/secrets/kubernetes.io/serviceaccount
      # Read this Pod's namespace
      NAMESPACE=$(cat ${SERVICEACCOUNT}/namespace)
      # Read the ServiceAccount bearer token
      TOKEN=$(cat ${SERVICEACCOUNT}/token)
      # Reference the internal certificate authority (CA)
      CACERT=${SERVICEACCOUNT}/ca.crt
      # Explore the API with TOKEN
      while [ "$(curl --cacert ${CACERT} --header "Authorization: Bearer ${TOKEN}" -X GET ${APISERVER}/api/v1/namespaces/default/pods?labelSelector=app=consul,component=server | jq '.items[].status.conditions[] | select(."type"=="Ready").status' | sed 's/"//g')" != "True" ]; do echo "waiting for consul server" && sleep 1; done
```
