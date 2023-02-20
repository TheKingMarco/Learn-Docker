# Start a container with a volume

```shell
docker run -d \
  --name devtest \
  --mount source=myvol2,target=/app \
  nginx:latest #avvia un container e crea un volume che verra montato in /app nel fs del container
docker volume ls
docker inspect myvol2 #inspect volume
docker exec -it devtest sh -c "echo hello > /app/file.txt" #creiamo un file all'interno del volume
docker exec -it devtest sh -c "ls /app && cat /app/file.txt" #mostra il file all'interno del container

#PERISTENZA DEI DATI

docker rm -f devtest #stop and delete container
docker ps -a #verifica che non ci sia devtest
docker volume ls #il volume esiste ancora
docker run -d \
  --name devtest \
  --mount source=myvol2,target=/app \
  nginx:latest #colleghiamo a devtest (altra istanza) il volume 
docker exec -it devtest sh -c "ls /app && cat /app/file.txt" #vediamo che il file esiste ancora, la persistenza funziona
```
# Populate a volume using a container
```shell
docker volume create nginx-vol
docker run -d \
  --name=nginxtest \
  --mount source=nginx-vol,destination=/usr/share/nginx/html \
  nginx:latest 
docker exec nginxtest sh -c "ls /usr/share/nginx/html"
docker rm -f nginxtest
docker run -it -d --name test --mount type=volume,source=nginx-vol,destination=/test nginx 
docker exec test sh -c "ls /test"
```

# Remove all
```shell
docker stop devtest
docker container rm devtest
docker volume rm myvol2

cd $HOME
sudo rm -fr mynginx

# Remove all
docker rm -f $(docker ps -a -q)
docker volume rm $(docker volume ls -q)
docker rmi $(docker images -q)
```
