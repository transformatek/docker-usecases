# Setting up Odoo on docker

This use case illustrate how to deploy odoo when building a webmapping application using microservices architecture based on docker container technology.

## Prerequisites

- a proper docker installation on the machine.
- [setup a PostGIS/PostgreSQL server on docker.](./postgis-on-docker.md) 

## Getting odoo image

Pull the image
```bash
docker pull odoo
```

## Create odoo user and grant superuser privileges

Connect to PostgeSQL server 
```bash
docker exec -it postgis psql -h localhost -U docker -d gis
```

Add an odoo admin user with **odoo** password
```sql
CREATE USER odoo;

ALTER USER odoo WITH SUPERUSER, CREATEDB, CREATEROLE;

-- Check privileges
\du

-- set odoo password as odoo
\password odoo

```

## Run the image

```bash
docker run -p 8069:8069 --name odoo --link postgis:db -t odoo
```

## Run Odoo with a custom configuration

```bash
docker run -v /path/to/config:/etc/odoo -p 8069:8069 --name odoo --link postgis:db -t odoo
```

## Mount custom addons

```bash
mkdir -p ~/odoo-addons && chmod -R ugo+rwx ~/odoo-addons
docker run -v $HOME/odoo-addons:/mnt/extra-addons -p 8069:8069 --name odoo --link postgis:db -t odoo
```
## with custom odoo password 

```bash
docker run -v $HOME/odoo-addons:/mnt/extra-addons -p 8069:8069 --name odoo -e POSTGRES_PASSWORD=<pwd> --link postgis:db -t odoo
```

Point the browser to [http://localhost:8069](http://localhost:8069) and setup a new database.

## Nginx configuration file sample

```apache
server {

        root /var/www/subdomain.domain.my/html;
        index index.html index.htm index.nginx-debian.html;

        server_name subdomain.domain.my;

        proxy_redirect off;

        location / {
            proxy_connect_timeout   3600;
            proxy_read_timeout      3600;
            proxy_send_timeout      3600;
            send_timeout            3600;

            proxy_pass http://127.0.0.1:8069/;
            proxy_pass_header Set-Cookie;
            proxy_set_header Host $host;
            proxy_set_header X-Forwarded-Proto $scheme;
            proxy_set_header X-Forwarded-Host $host;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        }

        location /longpolling {

        proxy_pass http://127.0.0.1:8072;
        
        }



        gzip on;
        gzip_min_length 1000;
}
```

## References : 

- [https://registry.hub.docker.com\_/odoo/](https://registry.hub.docker.com/\_/odoo/)
- [https://www.cybrosys.com/blog/how-to-configure-odoo-with-nginx-as-reverse-proxy](https://www.cybrosys.com/blog/how-to-configure-odoo-with-nginx-as-reverse-proxy)

