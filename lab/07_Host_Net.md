# host network
```shell
docker network ls
docker network inspect host
ip addr show
curl http://localhost:80 #non risponde nulla 
docker run -dit --network host --name webapp1 nginx
curl http://localhost:80 #risponde il nostro server ngnix
sudo netstat -tulpn | grep :80
docker stop webapp1
```
Cosi abbiamo visto che il nostro container avendo la stessa rete dell'host che lo ospita può visualizzare in locale la webapp

Vediamo invece se lo inseriamo in una net di tipo bridge

```shell
docker run -d --name webapp2 nginx
curl http://localhost:80 #vedi se risponde
docker stop webapp2
```
Vedremo un errore
Per esporre il servizio del container all'esterno senza utilizzare la rete dell'host possiamo publicare una posrta con il flag -p
```shell
docker run -d -p 8080:80 --name webbapp3 nginx
curl http://localhost:8080 #vedi se risponde
```
Vedremo che tutto funziona

# Remove all
```shell
docker rm -f $(docker ps -a -q)
docker volume rm $(docker volume ls -q)
docker rmi $(docker images -q)
```