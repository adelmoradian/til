# Redirects

Before a command is executed, its input and output may be redirected using a special notation interpreted by the shell.
The `>` and `>>` characters are redirect operators.
When you redirect output to a file, the `>` operator sends only the standard output to the file. Anything sent to standard error is not captured in the file.
To redirect standard to file:

```bash
command_does_not_exist 2> /dev/null
# the file descriptor 2 (standard error) is directed to a file called /dev/null.
```
When you use the standard redirection operator, you are implicitly using the file descriptor 1. Therefore these two commands are equivalent:

```bash
echo "contents of file1" 1> file1
echo "contents of file1" > file1
```

The number before the > symbol indicates the file descriptor to redirect. See [descriptors](descriptors.md) for more info.
The file /dev/null is a special kind of sink file created by Linux (and UNIX) kernels. It is effectively a black hole into which data can be pumped: anything written to it will be read in and ignored.
Therefore the `command_does_not_exist 2> /dev/null` command basically sends the errors to a black hole. Thsi command will not write any errors in the terminal. 

Another commonly seen redirection operator is 2>&1. By default standard output and standard error are redirected to the terminal.
What `2>&1` does is that it tells the shell to send the output on standard error (2) to whatever endpoint standard output is pointed to at that point in the command (&1).

The difference between pipe and redirect is:

- A pipe passes ‘standard output’ as the ‘standard input’ to another command
- A redirect sends output from a file descriptor to a file. Remember that there can be 3 file descriptors (stdout, stderr, stdin)

The `<` operator redirects standard input (file descriptor 0) _from_ a file _to_ the command.
Therefore `grep -c file < file1` is equivalent to `cat file1 | grep -c file`

## Examples

```bash
ping google.com # will redirect stdout and stderr to terminal
ping google.com 2> /dev/null # will redirect stdout to terminal and stderr to /dev/null
ping google.com 1> /dev/null 2> error.log # will redirect stdout to /dev/null and stderr to error.log
ping google.com >/dev/null 2>&1 # will redirect stderr to wherever stdout was at the time of execution (which is dev/null)

command_does_not_exist 2>&1 > outfile # will redirect both stdout and stderr to termianl because at the time of execution
# stdout is pointing to terminal

command_does_not_exist > outfile 2>&1 # will redirect both stdout and stderr to outfile because at the time of execution
# stdout is redirected to outfile
```
