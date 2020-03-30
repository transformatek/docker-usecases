# Configuration and running of PostGIS/PostgreSQL/Geoserver on docker

This use case illustrate how to build a webmapping application using microservices architecture based on docker container technology

## Getting the image of PostGIS

Pull the image
``` 
docker pull kartoza/postgis
```

Check that the image is installed
```
docker container ls 
```
Should have 
```
CONTAINER ID        IMAGE               COMMAND                  CREATED           STATUS        PORTS                     NAMES
aaa  kartoza/postgis     "/bin/sh -c /docker-…"   ..ago      ... min       0.0.0.0:25432->5432/tcp   postgis
```

Run the image
```
docker run --name "postgis" -p 25432:5432 -d -t kartoza/postgis
```

Connect with psql (make sure you first install postgresql client tools on your host / client)

```
psql -h localhost -U docker -p 25432 -l
```
**Note:** Default postgresql user is '**docker**' with password '**docker**'.

Connect with QGIS

![](./images/connect-with-qgis.png)


**More commands on the image website : ** [https://registry.hub.docker.com/r/kartoza/postgis](https://registry.hub.docker.com/r/kartoza/postgis)

