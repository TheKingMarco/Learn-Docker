# Running your first container

```shell
docker container run hello-world
```
# Docker Commands
```shell
docker ps -a #mostra tutti i container che hai creato
```
# Docker Images
```shell
docker image pull alpine #pull the image from the registry
docker image ls / docker images #list the images in our system
```
# Docker RUN
```shell
docker run alpine ls -l #run the container from the image and run a command inside
docker container run alpine echo "hello from alpine" 
docker container run -it alpine /bin/sh #-it (interactive terminal) ora sei dentro al container 
```

# Container Isolation

```shell
docker run -it --name C1 alpine /bin/sh #avvia un container in modalita interattica con il nome C1
    echo "hello world" > hello.txt #crea file 
    ls
    exit
docker container run alpine ls #vedi che non trovi il file che hai creato questo è un esempio di isolation
docker ps -a #mostra container
docker start C1 #startiamo il container C1 con il file creato
docker ps #vedi i container in esecuzione
docker exec C1 ls #lanciamo il comando all'interno del container C1 e vediamo il file
docker exec -it C1 /bin/sh #entriamo all'interno del container
```

# Remove all
```shell
docker rm -f $(docker ps -a -q)
docker volume rm $(docker volume ls -q)
docker rmi $(docker images -q)
```