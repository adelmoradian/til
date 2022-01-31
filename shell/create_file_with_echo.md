# Create file with echo

This is a simple command that creates a file with the argument of echo command.
It uses the redirection operator to create the file. 
If the file doesn’t exist it will be created. When using `>` the file will be overwritten, while the `>>` will append the output to the file.

## Examples

```bash
echo a > file1
cat file1
# returns
# a
echo b > file1
# returns
# b

echo a >> file2
cat file2
# returns
# a
echo b >> file2
cat file2
# returns 
# a
# b
```
