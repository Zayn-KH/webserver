# To check what is your docker network
```
docker network ls
```
Sample result:
```
root@ubuntu-s-4vcpu-8gb-amd-sgp1-01:~# docker network ls
NETWORK ID     NAME                   DRIVER    SCOPE
a2da4f795b86   bridge                 bridge    local
ea1edbe6608c   docker-stack_default   bridge    local
8365221234b4   host                   host      local
ed68b9267d8f   none                   null      local
```


# To inspect the network what container is connecting to it
```
docker network inspect docker-stack_default
```


