# AWS CLI query filter

Using the `--query` flag, you can do client side filtering (`--filter` gives you option for server side filtering).

Querying uses [JMESPath syntax](https://jmespath.org/tutorial.html)

- If you specify --output text, the output is paginated before the --query filter is applied, and the AWS CLI runs the query once on each page of the output. Due to this, the query includes the first matching element on each page which can result in unexpected extra output. To additionally filter the output, you can use other command line tools such as head or tail.
- If you specify --output json, --output yaml, or --output yaml-stream the output is completely processed as a single, native structure before the --query filter is applied. The AWS CLI runs the query only once against the entire structure, producing a filtered result that is then output.

## Examples

```bash
# Snapshot created after a specific date
aws ec2 describe-snapshots --owner self \
  --output json \
  query 'Snapshots[?StartTime>=`2018-02-07`].{Id:SnapshotId,VId:VolumeId,Size:VolumeSize}'

# output
# [
#     {
#         "id": "snap-0effb42b7a1b2c3d4",
#         "vid": "vol-0be9bb0bf12345678",
#         "Size": 8
#     }
# ]

# Most recent AMIs that you created
aws ec2 describe-images \
  --owners self \
  --query 'reverse(sort_by(Images,&CreationDate))[:5].{id:ImageId,date:CreationDate}'
```

For more examples see [AWS docs](https://docs.aws.amazon.com/cli/latest/userguide/cli-usage-filter.html)
