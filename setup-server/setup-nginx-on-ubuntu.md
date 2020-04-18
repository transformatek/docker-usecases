# Setup nginx on Ubunutu/Debian server

Location intelligence use cases using docker container.

## Make images work with nginx on custom DNS subdomains

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

Add configuration content. see [sample configuration files content](#Sample-configuration-file-content) for images specefic content.

```apache
server {
        listen 80;
        listen [::]:80;

        root /var/www/subdomain.domain.my/html;
        index index.html index.htm index.nginx-debian.html;

        server_name subdomain.domain.my;

        proxy_redirect off;

        location / {
            try_files $uri $uri/ =404;
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

## Sample configuration file content 

- [Configuration file content for Odoo](../setup-images-on-docker/odoo-on-docker.md#Nginx-configuration-file-sample).
- [Configuration file content for Geoserver](../setup-images-on-docker/geoserver-on-docker.md#Nginx-configuration-file-sample).


## More informations

- [https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-18-04#step-5-setting-up-server-blocks-(recommended)](https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-18-04#step-5-setting-up-server-blocks-(recommended))
- [https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-18-04](https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-18-04)
