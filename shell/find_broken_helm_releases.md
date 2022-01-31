# Find broken helm releases

```bash
helm ls -A -o json | jq  -r '.[] | select(.status != "deployed") | .name'
```
