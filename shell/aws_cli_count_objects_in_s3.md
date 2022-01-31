# AWS CLI count objects in s3

In case you want to check if a bucket or a path inside the bucket is empty or not.

```bash
totalFoundObjects=$(aws s3 ls s3://$1/$2/ --summarize | grep "Total Objects" | sed 's/[^0-9]*//g')
# the last bit is just to clean up the output
```
