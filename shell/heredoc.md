# Heredoc

Heredoc (AKA Here document) is a type of redirection that allows you to pass multiple lines of input to a command.
The syntax of writing HereDoc takes the following form:

```bash
[COMMAND] <<[-] 'DELIMITER'
  HERE-DOCUMENT
DELIMITER
```

- Starts with command (optional) and `<<` or `<` redirect operator
- Any delimiter can be used but EOF is very common
- If the delimiting is unquoted, the shell will substitute all variables, commands and special characters before passing the here-document lines to the command.
- Appending a minus sign (optional) to the redirection operator `<<-`, will cause all leading tab characters to be ignored.
- If you are using a heredoc inside a statement or loop, use the `<<-` redirection operation that allows you to indent your code.
- Instead of displaying the output on the screen you can redirect it to a file using the `>` or `>>` operators.
- If the file doesn’t exist it will be created. When using `>` the file will be overwritten, while the `>>` will append the output to the file.
- Seems like `<<EOF` and `<< EOF` is the same and the spacing doesn't mantter
- See [redirects](redirects.md) to learn more about `>` and `<` operators

## Examples

```bash
cat <<EOF
The current working directory is: $PWD
You are logged in as: $(whoami)
EOF
# output
# The current working directory is: path/to/current/dir
# You are logged in as: user

cat <<"EOF"
The current working directory is: $PWD
You are logged in as: $(whoami)
EOF
# output
# The current working directory is: $PWD
# You are logged in as: $(whoami)
EOF

cat <<EOF > file.txt
The current working directory is: $PWD
You are logged in as: $(whoami)
EOF
# writes the output to a file
# will create the file if it does not exist
# will override content is applicable

cat <<'EOF' |  sed 's/l/e/g'
Hello
World
EOF
# output
# Heeeo
# Wored

cat <<'EOF' |  sed 's/l/e/g' > file.txt
Hello
World
EOF
# << operator passes the heredoc to input of cat command
# then the output of cat is piped to sed
# and output of sed is redirected to file
```
