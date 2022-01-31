# Content References

```yaml
 ---
 values:
   - &ref Something to reuse
   - *ref      # Literal "Something to reuse" is inserted here!

---
 default_settings:
   install:
     dir: /usr/local
     owner: root
   config:
     enabled: false

 # Derive settings for 'my_app' from default and change install::owner
 # and add further setting "group: my_group"

 my_app_settings:
   <<: *default_settings
   install:
     owner: my_user
     group: my_group
```
