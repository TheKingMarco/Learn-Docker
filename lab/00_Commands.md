# Docker
## Common Lifecicle Container
```shell
docker pull <nome-img:tag> #scarica dal repository diriferimento l'immagine
docker run --name <name-cont> <img-name:tag> <command>#avvia istanza container con un determinato nome
docker run -it <img-name:tag> /bash #avvia istanza container in modalità (interactive terminal)
docker run -d -p <ipcont:portacont:portservizio> <img-name:tag> /bash #avvia istanza container in modalità detach (gira in background), e con esposizione di una porta (portacont).
docker start <cont-id-or-name> #fa partire un container che era stoppato
docker stop <cont-id-or-name>#stoppa un container
docker rm <cont-id-or-name> #rimuove un container stoppato
docker rm -f <cont-id-or-name> #stoppa e rimuove un container anche se in esecuzione
```
## Images
```shell
docker images #mostra immagini locali
docker rmi <nome-img>#rimuove immagine localmente
```
## Inspect
```shell
docker ps #mostra i container in running
docker ps -a #mostra tutte le istanze container create stoppate e non
docker inspect <cont-name> #ottieni maggiori info sul container
```

# Storage
## Volume
```shell
docker volume create <name-vol> #crea un volume
docker volume ls #lista dei volumi
docker volume inspect <name-vol> #maggiori informazione di un determinato volume
docker volume rm <name-vol> #rimuovi un volume
docker volume prune #rimuovi tutti i volumi inutilizzati
```

# Remove all
```shell
docker rm -f $(docker ps -a -q)
docker volume rm $(docker volume ls -q)
docker rmi $(docker images -q)
```