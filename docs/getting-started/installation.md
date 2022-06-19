# Getting started

## Installation

This package available in Packagist as `czernika/kawa`. As it still in development mode, you need to require master branch - no releases were made

### Install with Docker

You need [Docker](https://www.docker.com/) installed on your machine

> Note: dev-master branch in use!

```bash
docker run --rm --interactive --tty \
    --volume $PWD:/app \
    --user $(id -u):$(id -g) \
    composer create-project czernika/kawa:dev-master <project-dir>
```

## Running with Docker

Kawa Framework comes with Docker configuration

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

or go to `example.test` in your browser and fill in website data

Optional: install npm dependencies

```sh
docker exec -it kawa npm install
```
