# Containerize an application
## Get the app
```shell
git clone https://github.com/docker/getting-started.git
```
## Build the app’s container image
```shell
cd getting-started/app
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
docker build -t myImage:v1 .
```
## Start an app container
```shell
docker run --name todo -dp 3000:3000 myImage:v1
http://localhost:3000
#add some items
docker rm -f todo
```
# Update the application
```shell
#edit in line 56
vi src/static/js/app.js
docker build -t myImage:v2 .
docker run --name todo -dp 3000:3000 myImage:v2 #see the new update
```
# Share the application
```shell
docker tag myImage:v2 thekingmarco/ToDoImage:v1.0 #tagga imagine locale con docker account name
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
```

# Multi container apps
```shell

```


# Remove all
```shell
docker rm -f $(docker ps -a -q)
docker volume rm $(docker volume ls -q)
docker rmi $(docker images -q)
```