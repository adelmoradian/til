# List stopped containers

```bash
# list stopped containers
docker ps -a -f status=exited

# only their id
docker ps -a -f status=exited -q

# now you can cleanup!
```
