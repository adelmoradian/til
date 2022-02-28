# List ASG ARN of EKS nodegroups

```hcl
data "aws_eks_node_groups" "node_groups" {
  cluster_name = "demo"
}

data "aws_eks_node_group" "node_group" {
  count = length(data.aws_eks_node_groups.node_groups)

  cluster_name    = "demo"
  node_group_name =data.aws_eks_node_groups.node_groups.names[count.index]
}

data "aws_autoscaling_group" "asg" {
  count = length(data.aws_eks_node_groups.node_groups)
  name  = data.aws_eks_node_group.node_group[count.index].resources[0].autoscaling_groups[0].name
}
```
