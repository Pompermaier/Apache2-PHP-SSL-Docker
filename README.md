## Docker -- Apache+PHP with SSL via Let's Encrypt

Provides a docker image containing `apache2`, `PHP` and SSL certs generated by Let's Encrypt.

### Instructions

Run the container via:

```
docker run --rm \
    --restart always \
    -v ${DOCUMENT_ROOT}:/var/www/app \
    -v ${SITES_ENABLED}:/etc/apache2/sites-enabled \
    -p 80:80 \
    -p 443:443 \
    -e "DOMAINS=" \
    -e "WEBMASTER_MAIL=" \
    --name httpd-php-ssl \
    httpd-php-ssl:latest
```

### Configuration

- `${DOCUMENT_ROOT}` is the directory on the host containing HTML etc.
- `${SITES_ENABLED}` is a directory containing `.conf` files detailing virtual hosts for Apache.
- `DOMAINS` is a comma-seperated list of domains to fetch SSL certificates for.
- `WEBMASTER_MAIL` is self-explanatory.

### VirtualHost setup

Each `.conf` in `${SITES_ENABLED}` should follow the template:

```
<VirtualHost *:80>
    ServerName example.com
    ServerAlias example.com
    DocumentRoot /var/www/app
</VirtualHost>

<VirtualHost *:443>
    ServerName example.com
    ServerAlias example.com
    DocumentRoot /var/www/app

    SSLCertificateFile /etc/letsencrypt/live/example.com/fullchain.pem
    SSLCertificateKeyFile /etc/letsencrypt/live/example.com/privkey.pem
    Include /etc/letsencrypt/options-ssl-apache.conf
</VirtualHost>
```

### Extra details

See the following for details (after all, I based this container on them!):

- [https://github.com/romeOz/docker-apache-php](https://github.com/romeOz/docker-apache-php)
- [https://github.com/BirgerK/docker-apache-letsencrypt](https://github.com/BirgerK/docker-apache-letsencrypt)

### License

Licensed under the MIT license.
