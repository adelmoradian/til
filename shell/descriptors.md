# Descriptors

The ‘file descriptor’ is a number associated with the process that represents that ‘file’.
You can then write to that ‘file descriptor’ and the operating system will take care of ensuring the data goes to the right place in the appropriate way.

In UNIX/Linux each process gets three file descriptors by default. They are numbered zero to two:

- 0 is 'standard input'
- 1 is 'standard output'
- 2 is 'standard error'
