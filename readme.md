# Setup Handsup project with Docker

This example docker-compose setup is explicitly for handsup corporate backend project. However, you might find it useful to setup your own local environment for whatever you are building.

This stack is composed of **PHP@7.2**, **nginx**, **MySQL** and **Redis**.

## Why?

As I came across setting up local dev environment, installing PHP on macos is really a pain in the ass. It took me 3 days to complete the environment setup. Thought it would be useful to have a consistent environment setup guide for future new comers on handsup-api project.

## Clone this project to your root project directory

git clone `git@github.com:huangc28/handsup-docker-example.git`

## docker-compose.example.yml

Move `docker-compose.example.yml` to your `handsup-api` root project directory.

## SSL certificate

Create a local SSL certificate using [mkcert](https://github.com/FiloSottile/mkcert). you will get two files

  - `{{ YOUR_DOMAIN }}.key.pem`
  - `{{ YOUR_DOMAIN }}.pem`

Rename `{{ YOUR_DOMAIN }}.key.pem` to `{{ YOUR_DOMAIN }}.key` and place it in `docker/nginx`.
Rename `{{ YOUR_DOMAIN }}.pem` to `{{ YOUR_DOMAIN }}.crt` and place it in `docker/nginx`.

## public / private key

Move your `~/.ssh/id_rsa` and `~/.ssh/id_rsa.pub` to `docker/php`. The reason being that we need ssh key to pull dependencies from handsup private registry.

## .env.example

Copy and paste the content in `.env.example` file to your `handsup-api` project root `.env` file. The following env variables are needed to spin up `docker-compose` properly.

```
APP_PORT=8888
APP_TSL_PORT=443
SYSTEM_ENV=DOCKER
PROJECT_PATH=/var/www

MYSQL_SERVER=127.0.0.1
MYSQL_PORT=3306
MYSQL_ROOT_PASSWORD={{ YOUR_DB_PASSWORD }}

REDIS_SERVER=127.0.0.1
REDIS_PORT=6379
```

Copy and paste the content in `handsup-api/.env.example` file to your `handsup-api` project root `.env` file. Then modify the following env variables. You can generate App Key by `php artisan key:g`.

```
APP_KEY={{ YOUR_KEY }}

# DB_HOST=127.0.0.1
DB_HOST=mysql
DB_PORT=3306
DB_PASSWORD={{ YOUR_DB_PASSWORD }}

PERSISTENT_REDIS_HOST=redis
QUEUE_REDIS_HOST=redis
CACHE_REDIS_HOST=redis
```

Try `php artisan optimize:clear` if ENV doesn't update.

## spin up and run!

prompt `docker-compose up` to spin up the stack. prompt `http://localhost:8888` in the browser, you should see laravel index page. you are ready to go!

## setup Laravel

Move `develper.example.sh` to your `handsup-api` root project directory.
prompt `./develper.sh composer install` to install the framework's dependencies.
