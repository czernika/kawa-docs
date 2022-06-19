# Getting started

## Installation

TODO installation guide

## Running with Docker

Kawa Framework comes with Docker configuration. All you need is [Docker](https://www.docker.com/) installed on your machine

Kawa Framework pulls:

- PHP 8.0 with all necessary extensions for WordPress
- Mysql 8
- Nginx server
- Composer
- NodeJS
- Phpmyadmin panel
- Mailhog
- WP Cli utility

Up container

```bash
docker compose up -d
```

1. Fill in `.env` file
2. Create database with

```bash
docker exec -it kawa wp db create
```

3. Change domain name from `kawa.test` into desired one within `docker/nginx/nginx.conf`
4. Don't forget to add domain in `/etc/hosts` file

```txt
127.0.0.1 example.test
```

5. Finish core installation process

```bash
docker exec -it kawa wp core install --url=example.test --title=Example --admin_user=supervisor --admin_email=info@example.test --admin_password=strongpassword
```

5. Generate salts

TODO check does this works after changing default docker user

```bash
docker exec -it kawa wp dotenv salts generate
```

6. Install npm dependencies

```sh
docker exec -it kawa npm install
```
