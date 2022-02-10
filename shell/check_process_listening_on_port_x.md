# Check process listening on port x

We can do this using lsof utility.

lsof stands for list open files and that's all that it does.

`lsof -nP -i4TCP:2000 | grep LISTEN`
