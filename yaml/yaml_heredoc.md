# YAML heredoc

```yaml
# Block notation: Newlines become spaces
content:
   Arbitrary free text
   over multiple lines stopping only
   after the indentation changes...

# Literal style: Newlines are preserved
content: |
   Arbitrary free text            
   over "multiple lines" stopping 
   after indentation changes...  

# + indicator: Keep extra newlines after the block
content: |+                      
   Arbitrary free text with newlines after

# - indicator: remove extra newlines after block
content: |-
   Arbitrary free text without newlines after it

# folded style: folded newlines are preserved
content: >
   Arbitrary free text
   over "multiple lines" stopping
   after indentation changes...
```
