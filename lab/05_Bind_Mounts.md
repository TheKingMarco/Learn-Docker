# Bind Mounts

```shell
mkdir target-nginx #crea folder da ontare sul container
cd target-nginx
touch index.html
vi index.html
```
paste this on the index.html file
```html
<!DOCTYPE html> 
<html> 
<body> 

<h1>My First Heading</h1> 
<p>My first paragraph.</p> 

</body> 
</html>
```
start container
```shell
docker run -d \
  -p 3000:80 \
  --name devtest \
  --mount type=bind,source="$(pwd)",target=/usr/share/nginx/html \
  nginx:latest
curl localhost:3000 #vedi se risponde sulla il server web
docker exec -it devtest sh -c "ls /usr/share/nginx/html" #controlliamo che ci sia il file
docker exec -it devtest sh -c "touch /usr/share/nginx/html/file.txt" #aggiungiamo un file all'interno della cartella nel container
ls -la #vediamo se c'è anche nel nostro file system
```

# bind mount in read-only
```shell
cd ..
mkdir target-nginx-ro #crea folder da ontare sul container
cd target-nginx-ro
echo "hello" > file.txt #cra file all'interno della cartella
docker run -d \
  -it \
  --name devtestro \
  --mount type=bind,source="$(pwd)",target=/app,readonly \
  nginx:latest
docker exec -it devtestro sh -c "ls /app" #controlliamo che ci sia il file.txt
docker exec -it devtestro sh -c "touch /app/test.txt" #error
```
see the error

# Remove all
```shell
cd $HOME
rm -rf target-nginx-ro target-nginx
docker rm -f $(docker ps -a -q)
docker volume rm $(docker volume ls -q)
docker rmi $(docker images -q)
```