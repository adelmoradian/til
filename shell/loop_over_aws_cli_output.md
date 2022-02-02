# Loop over AWS CLI output

The key is to use `--output text` flag. 

When using this flag the output is paginated before the --query filter is applied, and the AWS CLI runs the query once on each page of the output. Due to this, the query includes the first matching element on each page which can result in unexpected extra output. To additionally filter the output, you can use other command line tools such as head or tail.

# Examples

```bash
for instance in $(aws autoscaling describe-auto-scaling-groups --auto-scaling-group-name foo --output text --query 'AutoScalingGroups[].Instances[].InstanceId'); do
  echo 'do stuff'
done
```
