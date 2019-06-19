
https://github.com/wsargent/docker-cheat-sheet

Na Windowsu umjesto singlequoute (') koristi doublequotes (")...

Image je kao EXECUTABLE, kod spreman za izvršavanje.
Container je kao PROCES, jedna instanca image.

Probati `docker run hello-world` čisto da znamo da fercera..  

Piše se `docker image ls` da izlista sve **image**.
Piše se `docker ps` da izlista sve **containere**.

Dockerfile defines what goes on in the environment inside your container.  
To se koristi kad ti praviš svoj image.

## Buildanje image
`docker build --tag=friendlyhello .`
Image je izbildan u lokalni image repo.

## Run
`docker run -p 4000:80 friendlyhello`

## Stop container
`docker stop friendlyhello` ili  
`docker stop tet354eg4535`

## Start stopped container 
`docker start friendlyhello`

## Delete image
docker image rm tet354eg4535


## Container info
`docker inspect tet354eg4535`  
`docker logs tet354eg4535`

`docker top`    ->  show running processes in container
`docker stats`  ->  show containers' resource usage statistics
`docker exec`   ->  execute a command in container
  `docker exec -it f9b299fca359 /bin/bash` (da "uđeš" u container)
`docker exec`   ->  execute a command in container

## Stop all containers
docker stop $(docker ps -aq)



## Docker file system
https://www.ionos.com/community/server-cloud-infrastructure/docker/understanding-and-managing-docker-container-volumes/

Doker image sadrži read-only FS.  
Kad se instancira container, napravi se read-write FS, sa svim fajlovima iz RO-FS kopiranim.  
Čim se ubije container, ubije se i taj RW-FS, zgodno pravo.

E sad, da ne bi stalno rekreirali te fajlove, koristimo tzv. VOLUMEs.  
Volume-i prežive između rekreiranja containera.  
Pomoću njih naš host može komunicirati sa containerom, dijeliti fajlove.  
Također, može se namjestit i da containeri između sebe dijele volume.

Kreiranje volume:
`docker volume create --name my-volume`  
Ovaj volume će bit kreiran negdje tamo.. kucaj `docker volume inspect my-volume`

Pokretanje containera sa namapiranim volume:
`docker run -it --name my-volume-test -v my-volume:/data`  
Ovako će container vidjeti u svom folderu `/data` sadržaj volumea `my-volume`.

### Bind Mount
Ovo je zgodno da nabrzakE testiraš nešto zDokerom.  
Namapiraš explicitno folder sa hosta na Dokerov folder.  
Ovo nije dobro jer se ne možeš oslonit da će svaki user imat taj folder spreman.. baš za ovaj naš docker container i tako to...  
Plus ne može se koristit u Dockerfile tek tako.