# Check if a package is installed as a condition

```bash
terraform --version >/dev/null 2>&1  || echo 'nothing will be pronted because i have terraform'

fake_package --version >/dev/null 2>&1  || echo 'this will be printed because i dont have fake_package'
# this will be printed because i dont have fake_package
```
Technically works also without `>/dev/null 2>&1` but using it basically silences everything so that we can use this in a script
