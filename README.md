# Docker Use Cases
Location intelligence use cases using docker container.

## Prerequisite 

- a proper docker installation on the machine.

Optional : 

- [setup a docker user defined bridge network](https://docs.docker.com/network/bridge/##differences-between-user-defined-bridges-and-the-default-bridge)

## Setup images on docker :

- [PostGIS/PostgreSQL Spatial DBMS](./setup-images-on-docker/postgis-on-docker.md) 
- [Odoo Entreprise management system](./setup-images-on-docker/odoo-on-docker.md) 
- [Geoserver webmapping server](./setup-images-on-docker/geoserver-on-docker.md) 


## Configuration of images to work with nginx on custom DNS subdomains

Create a directory to serve 
```bash
sudo mkdir -p /var/www/subdomain.domain.my/html
ls /var/www
sudo chown -R $USER:$USER /var/www/subdomain.domain.my/html
sudo chown -R 755 /var/www/subdomain.domain.my
```

Add a default webpage
```bash
cd  /var/www/subdomain.domain.my/html
sudo nano index.html
```

Add default html content
```html
<html>
    <head>
        <title>Welcome to subdomain.domain.my!</title>
    </head>
    <body>
        <h1>Success!  The subdomain.domain.my server block is working!</h1>
    </body>
</html>
```

Create a configuration file
```
cd /etc/nginx/sites-available/
sudo touch subdomain.domain.my 
sudo nano subdomain.domain.my 
```

Add configuration content
```apache
server {

        root /var/www/subdomain.domain.my/html;
        index index.html index.htm index.nginx-debian.html;

        server_name subdomain.domain.my;

        proxy_redirect off;

        location / {
            proxy_pass http://127.0.0.1:8069/;
            proxy_pass_header Set-Cookie;
            proxy_set_header Host $host;
            proxy_set_header X-Forwarded-Proto $scheme;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        }

}
```

Enable the site 

```bash
sudo ln -s /etc/nginx/sites-available/subdomain.domain.my /etc/nginx/sites-enabled/
sudo ls /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl restart nginx
```

Make sure your **subdomain is setup** and run Certbot to enable SSL 

```bash
sudo ufw status
sudo certbot --nginx -d subdomain.domain.my
sudo certbot renew --dry-run
```

For more information :
- [https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-18-04#step-5-setting-up-server-blocks-(recommended)](https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-18-04#step-5-setting-up-server-blocks-(recommended))
- [https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-18-04](https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-18-04)
