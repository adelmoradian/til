# MYSQL pod fails with --initialize specified but the data directory has files in it

A new ext4 disk partition is not usually empty; there is a lost+found directory, which mysql is known to choke on. You could try adding --ignore-db-dir=lost+found to the CMD to know for sure (from mysql docs)

Here's an extract of my working YAML definition:

```yaml
name: mysql-master
image: mysql:5.7
args:
  - "--ignore-db-dir=lost+found"
```
