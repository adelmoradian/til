# Memory and CPU usage

```bash
free -h # see free memory
top -o %MEM # top memory consuming procesess
top -o %MEM | head -n 15 # top 15
ps aux # all running proceses 
ps auxfww # all running processes with full command
ps aux --sort -%mem # top running processes by mem usage
ps aux --sort -%cpu | head -10 # top 10 sort by cpu
ps -eo pid,tid,class,rtprio,stat,vsz,rss,comm --sort -rss # sort processes by rss
ps -eo pid,%mem,%cpu,tid,class,rtprio,stat,vsz,rss,comm --sort -%mem | head -10
# what is rss and vsz? https://stackoverflow.com/questions/7880784/what-is-rss-and-vsz-in-linux-memory-management
# Rss: called RSS in ps and RES in top, is the sum of memory that is mapped to physical pages of memory.
# This gets closer to the actual memory budget of the process, but there is a problem,
# if you add up the Rss of all the processes, you will get an overestimate the memory in use because some pages will be shared.
```
