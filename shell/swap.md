# Swap

Linux divides its physical RAM (random access memory) into chucks of memory called pages.
Swapping is the process whereby a page of memory is copied to the preconfigured space on the hard disk, called swap space, to free up that page of memory.
The combined sizes of the physical memory and the swap space is the amount of virtual memory available.

Swapping is necessary for two important reasons.
First, when the system requires more memory than is physically available, the kernel swaps out less used pages and gives memory to the current application (process) that needs the memory immediately.
Second, a significant number of the pages used by an application during its startup phase may only be used for initialization and then never used again. The system can swap out those pages and free the memory for other applications or even for the disk cache.

However, swapping does have a downside. Compared to memory, disks are very slow.
Memory speeds can be measured in nanoseconds, while disks are measured in milliseconds, so accessing the disk can be tens of thousands times slower than accessing physical memory.
The more swapping that occurs, the slower your system will be. Sometimes excessive swapping or thrashing occurs where a page is swapped out and then very soon swapped in and then swapped out again and so on. In such situations the system is struggling to find free memory and keep applications running at the same time. In this case only adding more RAM will help.

```bash
swapon --show # show swap
df -h # show disks
fallocate -l 1G /swapfile # create swap file of 1gb in the file /swapfile.
# /swapfile will get created in this process (file doesn't need to be there to beging with)
chmod 600 /swapfile # secure it if you wanna use it as swap
mkswap /swapfile # mark the file as swap space
swapon /swapfile # start using this swap file
cp /etc/fstab /etc/fstab.bak && echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab # make swap file permanent after reboot
cat /proc/sys/vm/swappiness # see current swappiness
sysctl vm.swappiness=10 # set swappiness to 10... 
# add vm.swappiness=10 to the end of /etc/sysctl.conf to make this change persiste after reboot
cat /proc/sys/vm/vfs_cache_pressure # see current vfs cache pressure
sysctl vm.vfs_cache_pressure=50 # set current vfs cache pressure
# add vm.vfs_cache_pressure=50 to the end of /etc/sysctl.conf to make this change persiste after reboot

# to rollback do the following
swapoff -v /swapfile # to turn off swap
# Remove entry '/swapfile none swap sw 0 0' from /etc/fstab
rm /swapfile
```
