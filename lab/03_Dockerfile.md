# Simple App with Dockerfile


```shell
mkdir mynginx
cd mynginx
mkdir static-html-directory
cd static-html-directory
touch index.html
vi index.html
```
inserisci questo codice html nel file index.html
```shell
<!DOCTYPE html>
<html>
  <head>
    <title>My Web Page</title>
  </head>
  <body>
    <h1>Welcome to my web page!</h1>
    <p>This is a simple web page created with HTML.</p>
  </body>
</html>
```
crea Dockerfile
```shell
cd ..
touch Dockerfile 
vi Dockerfile
```
crea Dockerfile
```shell
FROM nginx   #start with a nginx images  
COPY static-html-directory /usr/share/nginx/html #copy or dir in container dir
```
Build
```shell
docker build -t my-nginx-img .    # -t : Name and optionally a tag in the 'name:tag' format, defaul is latest
docker images
```
Run Container con la nostra app
```shell
docker run --name some-nginx -d -p 8080:80 my-nginx-img  #Exposing external port
docker ps -a     #Show all container (exit and running)
curl http://localhost:8080     #Test web application
```

# First Contenairized app with Dockerfile
In questo tutorial vedremo come contenirazzare una semplice app "Hello World" (Node.js) con Dockerfile

Create a new directory for your project and navigate to it:
```shell
mkdir my-app
cd my-app
```
Create a file named Dockerfile in your project directory
```shell
vi Dockerfile
```
Open the Dockerfile in a text editor and add the following code:
```shell
# Use an official Node.js runtime as a parent image
FROM node:14-alpine

# Set the working directory to /app
WORKDIR /app

# Copy all to the working directory
COPY . .

# Install dependencies
RUN npm install

# Copy the rest of the application code to the working directory
COPY . .

# Expose port 3000
EXPOSE 3000

# Start the application
CMD ["npm", "start"]
```
Create a file named index.js in your project directory and add the following code:
```shell
vi index.js #creazione file
#app "Hello World"
const http = require('http');

const hostname = '0.0.0.0';
const port = 3000;

const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello, World!\n');
});

server.listen(port, hostname, () => {
  console.log(`Server running at http://${hostname}:${port}/`);
});
```
Create a file named package.json in your project directory and add the following code:
```shell
vi package.json
#package.json
{
  "name": "my-app",
  "version": "1.0.0",
  "description": "A simple 'Hello, World!' Node.js application",
  "scripts": {
    "start": "node index.js"
  },
  "dependencies": {
    "http": "^0.0.1"
  }
}
```

This file describes your application and its dependencies. The "start" script starts the application by running node index.js.

Build the Docker image:
```shell
docker build -t my-app:v1 . #build the app with name and tag
```
This command builds a Docker image named my-app using the Dockerfile in the current directory.

Run the Docker container:
```shell
docker run -p 3000:3000 my-app
```
# Remove all
```shell
docker rm -f $(docker ps -a -q)
docker volume rm $(docker volume ls -q)
docker rmi $(docker images -q)
```
