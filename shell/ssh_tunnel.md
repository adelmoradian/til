# SSH Tunnel

SSH tunneling or SSH port forwarding is a method of creating an encrypted SSH connection between a client and a server machine through which services ports can be relayed.

# Example

```bash
# start the ssh agent
eval `ssh-agent -s`

# add your private key to ssh agent
ssh-add private_key.pem

# ssh-keyscan is a utility for gathering the public ssh host keys of a number of hosts.
# It was designed to aid in building and verifying ssh_known_hosts files. 
# -H is to Hash all hostnames and addresses in the output. Hashed names may be used normally by ssh and 
# sshd, but they do not reveal identifying information should the file's contents be disclosed. 
ssh-keyscan -H <ssh server ip> >> /root/.ssh/known_hosts

# -4 Forces ssh to use IPv4 addresses only.
# -N Do not execute a remote command.  This is useful for just forwarding ports. i.e. not actually getting into the server
# -f Requests ssh to go to background just before command execution.  This is useful if ssh is going to ask for passwords or passphrases, but the user wants it in the background.  This implies -n.
# -n Redirects stdin from /dev/null(actually, prevents reading from stdin).  This must be used when ssh is run in the background
# -L Specifies that connections to the given TCP port or Unix socket on the local (client) host are to be forwarded to the given host and port, or Unix socket, on the remote side.
ssh -4 -f user@<ssh server ip> -L 1236:<destination host or ip>:3306 -N -i private_key.pem
```
