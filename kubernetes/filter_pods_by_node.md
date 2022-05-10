# Filter pods by node

``kg pods --all-namespaces -o=json | jq '.items[] | select(.spec.nodeName == "ip-10-221-4-200.eu-central-1.compute.internal") | .metadata.name'
