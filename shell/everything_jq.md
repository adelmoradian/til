# Everything jq

```bash
cat <<EOF > file.json
{
    "timestamp": 1234567890,
    "report": "Age Report",
    "results": [
        { "name": "John", "age": 43, "city": "TownA" },
        { "name": "Joe",  "age": 10, "city": "TownB" }
    ]
}
EOF

# Output formatting
jq . cat file.josn

# To extract top level attributes “timestamp” and “report”
jq '. | {timestamp,report}'

# To extract name and age of each “results” item
jq '.results[] | {name, age}'

# To extract name and age as text values instead of JSON
jq -r '.results[] | {name, age} | join(" ")'

# Filter by attribute
jq '.results[] | select(.name == "John") | {age}'          # Get age for 'John'
jq '.results[] | select((.name == "Joe") and (.age = 10))' # Get complete records for all 'Joe' aged 10
jq '.results[] | select(.name | contains("Jo"))'           # Get complete records for all names with 'Jo'
jq '.results[] | select(.name | test("Joe\s+Smith"))'      # Get complete records for all names matching PCRE regex 'Joe\+Smith'

# If you want to combine subkeys at different levels 
# jq '.items[] | .metadata["created"], .name'
# The drawback being, that you do not get a JSON output, but each value on a new line.
jq '.results[].name, .report'

# Adding a new field and make the result looks like this
# {
#   "timestamp": 1234567890,
#   "report": "Age Report",
#   "results": [
#     {
#       "name": "John",
#       "age": 43,
#       "city": "TownA"
#     },
#     {
#       "name": "Joe",
#       "age": 10,
#       "city": "TownB"
#     }
#   ],
#   "c": 3
# }
jq '. |= . + {"c": 3}'

# Adding elements to list

echo '{ "names": ["Marie", "Sophie"] }' |\
jq '.names |= .+ [
   "Natalie"
]'  

# When you want to iterate over an array, and the array you access is empty you get something like
jq: error (at <stdin>:3): Cannot iterate over null (null)
# To workaround the optional array protect the access with
select(.my_array | length > 0)

# types
echo '[true, null, 42, "hello", []]' | jq 'map(type)'
#results ["boolean","null","number","string","array"]

# to export env variables from jq
export $(jq -r '@sh "FOO=(.foo) BAZ=(.baz)"')

# Replace a value
jq --arg newval "$var" '(.report) |= sub("Age Report"; $newval)' file.json
# or a more elaborate example
jq --arg newval 'yyy' '(.spec.template.spec.containers[].env[] | select(.name == "ISTIO_META_OWNER").value) |= sub("[^/]*$"; $newval)' file.json
```

From [Lars Windolf's blog](https://lzone.de/cheat-sheet/jq)
