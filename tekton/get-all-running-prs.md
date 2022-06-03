# Get all running PipelineRuns

`tkn pr list -A -o json | jq '.items[].status.conditions[] | select(.reason== "Running")'`
