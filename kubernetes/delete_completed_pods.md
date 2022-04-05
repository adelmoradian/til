# Delete completed pods

You can get the list of completed pods with `kubectl get pod --field-selector=status.phase==Succeeded`
Or you can get the list of pods that completed with error with `kubectl get pod --field-selector=status.phase==Failed`

Then you can delete it with kubectl.

If this happened because of your own cron job you can set the `spec.failedJobsHistoryLimit` and `spec.successfulJobsHistoryLimit`
