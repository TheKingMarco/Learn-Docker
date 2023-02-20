# Containerize an application
## Get the app
```shell
git clone https://github.com/docker/getting-started.git
```
## Build the app’s container image
```shell
cd getting-started/app
ls
vi Dockerfile
```
insert this:
```shell
# syntax=docker/dockerfile:1
   
FROM node:18-alpine
WORKDIR /app
COPY . .
RUN yarn install --production
CMD ["node", "src/index.js"]
EXPOSE 3000
```
Build the Image
```shell
docker build -t myimage:v1 .
```
## Start an app container
```shell
docker run --name todo -dp 3000:3000 myimage:v1
curl http://localhost:3000
http://localhost:3000
#add some items
docker rm -f todo
```
# Update the application
```shell
#edit in line 56
vi src/static/js/app.js
docker build -t myimage:v2 .
docker run --name todo -dp 3000:3000 myimage:v2 #see the new update
```
# Share the application
```shell
docker tag myimage:v2 thekingmarco/ToDoImage:v1.0 #tagga imagine locale con docker account name
docker images
docker login #accedi al tuo account docker hub username e password
docker push thekingmarco/ToDoImage:v1.0 #push dell'imagine locale su docker hub repository
docker ps
docker images
docker rm -f $(docker ps -a -q)
docker rmi $(docker images -q)
docker pull thekingmarco/ToDoImage:v1.0
docker images
docker logout 
```
# Persist the App Data
By default, the todo app stores its data in a SQLite database at /etc/todos/todo.db
```shell
docker volume create todo-db
docker run --name todo -dp 3000:3000 --mount type=volume,src=todo-db,target=/etc/todos thekingmarco/ToDoImage:v1.0
http://localhost:3000 
#aggiungi elementi alla lista
docker rm -f todo
docker ps
docker run --name todo -dp 3000:3000 --mount type=volume,src=todo-db,target=/etc/todos thekingmarco/ToDoImage:v1.0
http://localhost:3000 #vedremo gli stessi elementi di prima
docker rm -f todo
```

# Multi container apps
now we create a mysql container in the same network of the our app , so they can comunicate and we can store the date of the our app in a mysql DB
```shell
docker network create net-todo-app #create a network
```
start mysql database with in the network with some enviroment variables, and create also a new volume that point to the folder where mysql store its data
```shell
docker run -d \
     --network net-todo-app --network-alias net-mysql \
     --name mysql \
     -v todo-mysql-data:/var/lib/mysql \
     -e MYSQL_ROOT_PASSWORD=toor \
     -e MYSQL_DATABASE=todos \
     mysql:8.0

docker exec -it mysql net-mysql -u root -p toor #connect to container
```
adesso che siamo dentro al conteiner lanciamo i seguwnti comandi per verificare la creazione del nostro db
```shell
SHOW DATABASES; 
exit
```
possiamo verificare il raggiungimento del db sulla rete attraverso un container specifico per troobleshoting di network
```shell
docker run --name trouble -it --network net-todo-app nicolaka/netshoot
    dig net-mysql #useful DNS tool , cerchiamo attraverso l'alias che abbiamo ipostato nel container
docker rm -f trouble
```
## Run your app with MySQL
The todo app supports the setting of a few environment variables to specify MySQL connection settings. They are:

MYSQL_HOST - the hostname for the running MySQL server
MYSQL_USER - the username to use for the connection
MYSQL_PASSWORD - the password to use for the connection
MYSQL_DB - the database to use once connected
```shell
docker exec -it mysql net-mysql -u root -p toor #entra in mysql
    ALTER USER 'root' IDENTIFIED WITH mysql_native_password BY 'secret';
    flush privileges;
```
start the container app
```shell
#-w si sposta nella work directory /app
 docker run -dp 3000:3000 \
   --name todo-app
   -w /app \
   --network net-todo-app \
   -e MYSQL_HOST=mysql \
   -e MYSQL_USER=root \
   -e MYSQL_PASSWORD=secret \
   -e MYSQL_DB=todos \
   thekingmarco/ToDoImage:v1.0 \

docker logs -f todo-app
http://localhost:3000 #add few items
docker exec -it mysql net-mysql -u root -p toor #entra nel db
    select * from todo_items #verifica i nuovi items
```


# Remove all
```shell
docker rm -f $(docker ps -a -q)
docker volume rm $(docker volume ls -q)
docker rmi $(docker images -q)
```