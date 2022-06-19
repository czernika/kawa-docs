# Installation

## Running with Docker

Kawa Framework comes with Docker configuration. All you need is Docker installed on your machine

Build container with

```sh
docker compose up -d
```

1. Fill in `.env` file
2. Create database

```sh
docker exec -it kawa-php wp db create
```

3. Change domain in `docker/nginx/nginx.conf`
4. Finish core installation process

```sh
docker exec -it kawa-php wp core install --url=example.com --title=Example --admin_user=supervisor --admin_email=info@example.com --admin_password=strongpassword
```

5. Generate salts

```sh
docker exec -it kawa-php wp dotenv salts generate
```

6. Install npm dependencies

```sh
docker exec -it kawa-php npm install
```
