# Convert JSON to YAML

```bash
ruby -ryaml -rjson -e 'puts YAML.dump(JSON.parse(STDIN.read))' <input.json
```
