#  default bridge network
```shell
docker network ls #mostra le resti presenti
docker network inspect bridge #vediamo i dettagli della rete bridge di default
```
Proviamo a connettere due container nella stessa rete bridge
```shell
#crazione due container in modalità detach 
docker run -dit --name alpine1 alpine ash 
docker run -dit --name alpine2 alpine ash
docker ps
docker attach alpine1 #colleghiamoci al container
    ip addr show #per vedere le schede di rete e notiamo l'ip che ha ricevuto
    ping -c 2 google.com #vediamo se raggiungiamo internet
    ctrl + p / ctrl + q
```
Adesso in un altra scheda colleghiamoci al secondo container
```shell
docker ps
docker attach alpine2 #colleghiamoci al container
    ip addr show #per vedere le schede di rete e notiamo l'ip che ha ricevuto
    ping -c 2 google.com #vediamo se raggiungiamo internet
    ctrl + p / ctrl + q
```
Proviamo la comunicazione tra i due container
Notiamo che solo la risoluzione tramite ip funziona, quindi nella default bridge la risoluzione DNS non funziona a differenza della User-Bridge Network
```shell
#alpine1
 ping -c 2 172.17.0.3
 ping -c 2 alpine2 #error
 ctrl + p / ctrl + q
```
chiudi tutte le sessioni con il comando exit
ps: Se non vuoi far terminare il container con exit clicca la sequanza di tast (ctrl + p / ctrl + q) in sequenza
# user-defined bridge networks
```shell
docker network create --driver bridge alpine-net #creazione di una network bridge custom
docker network ls #see the network
docker network inspect alpine-net
```
Adesso creiamo dei container collegati alla nostra net custom
```shell
docker run -dit --name alpine3 --network alpine-net alpine ash
docker run -dit --name alpine4 --network alpine-net alpine ash
docker run -dit --name jolly --network alpine-net alpine ash
docker network connect bridge jolly #colleghiamo la rete bridge di default al container jolly
```
Guarda le differente delle network
Adesso avremo i primi due container collegati alla net bridge
e i container 3 e 4 collegati alla notra rete custom
in più avremo il container jolly collegato ad entrambe le reti
```shell
docker ps -a
docker network ls
docker network inspect bridge
docker network inspect alpine-net
```
Dividiamo lo schermo con i quatro terminali e facciamo i test
```shell
#alpine1
docker attach alpine1
ping -c 2 google.com
ping -c 2 alpine2 #funziano solo con ip 
ping -c 2 172.17.0.3
ping -c 2 alpine3 #non funziona perche sono su reti diverse
ping -c 2 172.19.0.2 #non funziona perche sono su reti diverse
ping -c 2 alpine4 #non funziona perche sono su reti diverse
ping -c 2 jolly #funziano solo con ip 
ping -c 2 172.17.0.4 #funziona
ping -c 2 172.19.0.4 #non funziona rete diversa
ctrl + p / ctrl + q
#alpine3
docker attach alpine3
ping -c 2 google.com
ping -c 2 alpine1 #non funziona perche su reti diverse
ping -c 2 alpine2 #non funziona perche su reti diverse
ping -c 2 alpine4 #funziona sia con ip che con fqdn
ping -c 2 jolly 
ctrl + p / ctrl + q
```
Adesso vediamo il nostro container jolly che dovrebbe comunicare con tutti
```shell
#jolly
docker attach jolly
ping -c 2 google.com
ping -c 2 172.17.0.2 #alpine1
ping -c 2 172.17.0.3 #alpine2
ping -c 2 alpine3
ping -c 2 alpine4
ctrl + p / ctrl + q
```
# Remove all
```shell
docker rm -f $(docker ps -a -q)
docker volume rm $(docker volume ls -q)
docker network rm alpine-net
docker rmi $(docker images -q)
```