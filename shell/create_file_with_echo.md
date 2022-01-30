# Create file with echo

This is a simple command that creates a file with the argument of echo command.
It uses the redirection operator to create the file. 
- `>` will replace the content if the file already exists
- `>>` will create a new line

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
```bash
