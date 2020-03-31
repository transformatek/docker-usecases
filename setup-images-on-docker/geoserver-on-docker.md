# Setting up Geoserver on docker

This use case illustrate how to deploy geoserver when building a webmapping application using microservices architecture based on docker container technology.

## Prerequisite 

- proper docker installation on the machine.

Optional :

- [Setup a PostGIS/PostgreSQL server on docker.](./postgis-on-docker.md) 

## Getting Geosever image

Pull the image
``` 
docker pull geonode/geoserver:2.15.x
```

Download [https://build.geo-solutions.it/geonode/geoserver/latest/data-2.15.x.zip](https://build.geo-solutions.it/geonode/geoserver/latest/data-2.15.x.zip) and unzip it on $HOME/data

Run the image
```
docker run --name "geoserver" -v /var/run/docker.sock:/var/run/docker.sock -v $HOME/data:/geoserver_data/data -d -p 8080:8080 geonode/geoserver:2.15.x
```

Point the browser to [http://localhost:8080/geoserver](http://localhost:8080/geoserver) and login using **admin/geoserver**


## References : 

- [https://github.com/GeoNode/geoserver-docker](https://github.com/GeoNode/geoserver-docker)

