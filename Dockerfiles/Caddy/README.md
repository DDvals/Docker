# Caddy Server with PHP Docker image

Caddy server with PHP Docker image based on Alpine Linux 3.3 inspired by
stevepacker/caddy-php and abiosoft/caddy images

This image expose 80 and 443 ports and uses a base configuration file CaddyFile
but you can provide your CaddyFile.

## Build
To build put `Dockefile`, `CaddyFile` and `php-fpm-wrapper.sh` in a directory and run
```
sudo docker build -t <image_tag> .
```

## Run
To run this image launch
```
sudo docker run -d -p 80:80 -p 443:443 --restart=always --name=<container_name> \
  -v <site_folder>:/srv <image_tag>
```

In this case Caddy will use the standard CaddyFile and (if used) store SSL
certificates inside the container.

The best way to use Caddy is mount all directories and provide a CaddyFile
```sh
sudo docker run -d -p 80:80 -p 443:443 \
  --restart=always --name=<container_name> \
  -v <site_folder>:/srv \
  -v <caddyfile_dir>/CaddyFile:/etc/CaddyFile \
  -v <certs_dir>:/root/.caddy \
  <image_tag> --conf="/etc/CaddyFile"
```

If your php webapp needs to write inside the `/srv` directory (eg. Dokuwiki) you
have to change the owner of the directory of your app
`sudo docker exec <container_name> chown -R php:users /srv/<app_dir>`

## Docker Environment Variables

- `PHP_BEFORE`: command(s) to run prior to running `composer install` and `php-fpm`
- `PHP_COMPOSER_FLAGS`: flags to include with `composer install` (eg: --prefer-dist --no-dev)
- `PHP_AFTER`: command(s) to run after `composer install` and `php-fpm`

## Caddy Extensions:

- git
- ipfilter

## PHP Extensions:

- composer
- php-fpm
- php-cli
- php-curl
- php-gd
- php-json
- php-iconv
- php-mcrypt
- php-intl
- php-ctype
- php-pdo_mysql
- php-pdo_sqlite
- php-posix
- php-sockets
- php-imap
- php-openssl
- php-phar
- php-xml
- php-ldap
