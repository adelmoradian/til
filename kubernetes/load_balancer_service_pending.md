# Load balancer service pending

If we try to create a LoadBalancer on an AWS EKS cluster without any public subnet it will get stuck on the pending state and we won't get any external IP/DNS name for it.

The actual error is `Error syncing load balancer: failed to ensure load balancer: could not find any suitable subnets for creating the ELB`

By default AWS EKS only attaches load balancers to public subnets.
Since we might want to expose it to the internal subnet we can force to provision the LoadBalancer on the private subnet by adding an annotation to the **Service**

`service.beta.kubernetes.io/aws-load-balancer-internal: "true"`

to do it with kubectl run `kubectl annotate svc demo-lb -n pet2cattle "service.beta.kubernetes.io/aws-load-balancer-internal"="true" service/demo-lb annotated`

Sometimes you want to run a LB on a public subnet but get the same error. Then make sure that the following annotation exists on your public subnets:

`key: kubernetes.io/cluster/cluster-name`
`value: shared` or `owned`

More info [here](https://docs.aws.amazon.com/eks/latest/userguide/network-load-balancing.html)
