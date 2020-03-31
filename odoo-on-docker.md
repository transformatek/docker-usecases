# Setting up Geoserver on docker

This use case illustrate how to deploy odoo when building a webmapping application using microservices architecture based on docker container technology.

## Prerequisites

- a proper docker installation on the machine.
- [setup a PostGIS/PostgreSQL server on docker.](./postgis-docker.md) 

## Getting odoo image

Pull the image
``` 
docker pull odoo
```

## Create odoo user and grant superuser privileges

Connect to PostgeSQL server 
```
docker exec -it postgis psql -h localhost -U docker -d gis
```

Add an odoo admin user with **odoo** password
```
CREATE USER odoo;

ALTER USER odoo WITH SUPERUSER, CREATEDB, CREATEROLE;

-- Check privileges
\du

-- set odoo password as odoo
\password odoo

```

## Run the image

```
docker run -p 8069:8069 --name odoo --link postgis:db -t odoo
```

Point the browser to [http://localhost:8069](http://localhost:8069) and setup a new database.


## References : 

- [https://registry.hub.docker.com/_/odoo/](https://registry.hub.docker.com/_/odoo/)

