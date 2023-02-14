# Image creation from a container
```shell
docker run --name test -ti ubuntu bash 
    apt-get update
    apt-get install -y figlet #stampa in ASCII la stringa che gli passi
    figlet "hello docker"
    exit
docker container ls -a #get id container
docker container diff test# #mostra le differenze tra l'imagine iniziale e ciò che è stato modificato nel container
docker commit test #crea un immagine localmente dal container ce gli passiamo
docker images #vedi la nuova immagine e get image id
dokcer image tag <image-id> myfiglet #rinomina l'immagine e o assegna un tag all'immagine
docker images
docker run myfiglet figlet hello #lanciamo un container con la nostra nuova immagine
```
# publish to docker hub
```shell
docker tag myfiglet thkingmarco/myfiglet:v1.0 #tagga imagine locale con docker account name
docker images
docker login #accedi al tuo account docker hub username e password
docker push thkingmarco/myfiglet:v1.0 #push dell'imagine locale su docker hub repository
docker logout 
```
# Remove all
```shell
docker rm -f $(docker ps -a -q)
docker volume rm $(docker volume ls -q)
docker rmi $(docker images -q)
```
