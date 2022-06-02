# Requirements

Your web server should meet WordPress [requirements](https://wordpress.org/about/requirements/) to be able to install its core

!> Kawa Framework uses PHP 8.0 and does **NOT** support earlier versions of PHP

- [MySQL](https://www.mysql.com/) version 5.7 or greater OR [MariaDB](https://mariadb.org/) version 10.3 or greater.
- [HTTPS](https://wordpress.org/news/2016/12/moving-toward-ssl/) support
- [Composer](https://getcomposer.org/) dependency manager

The PHP extensions listed below are required for a WordPress site to work.

- json
- curl
- dom
- exif
- fileinfo
- hash
- imagick
- mbstring
- openssl
- pcre
- xml
- zip

Optional extensions can be found [here](https://make.wordpress.org/hosting/handbook/server-environment/)

## Server configuration

Bedrock folder structure [requires](https://docs.roots.io/bedrock/master/server-configuration/) your document root to be pointed into `web` directory

### Nginx

```
server {
  listen 80;
  server_name example.com;

  root /srv/www/example.com/web;
  index index.php index.htm index.html;

  # Prevent PHP scripts from being executed inside the uploads folder.
  location ~* /app/uploads/.*.php$ {
    deny all;
  }

  location / {
    try_files $uri $uri/ /index.php?$args;
  }
}
```

### Apache

```
<VirtualHost *:80>
        DocumentRoot /var/www/html/bedrock/web

        DirectoryIndex index.php index.html index.htm

        <Directory /var/www/html/bedrock/web>
            Options -Indexes

            # .htaccess isn't required if you include this
            <IfModule mod_rewrite.c>
                RewriteEngine On
                RewriteBase /
                RewriteRule ^index.php$ - [L]
                RewriteCond %{REQUEST_FILENAME} !-f
                RewriteCond %{REQUEST_FILENAME} !-d
                RewriteRule . /index.php [L]
            </IfModule>
        </Directory>
</VirtualHost>
```

You may also use `.htaccess` file for hosted web sites with the following content

```
RewriteEngine on

RewriteCond %{REQUEST_URI} !web/
RewriteRule ^(.*)$ /web/$1 [L]
```

## WP Cli

It is highly recommended to install [wp-cli](https://wp-cli.org) utility