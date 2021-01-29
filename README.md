# CakePHP Docker

[![Build](https://github.com/cnizzardini/cakephp-docker/workflows/Build/badge.svg?branch=master)](https://github.com/cnizzardini/cakephp-docker/actions)
[![CakePHP](https://img.shields.io/badge/cakephp-4.2-red?logo=cakephp)](https://book.cakephp.org/4/en/index.html)
[![Docker](https://img.shields.io/badge/docker-0db7ed.svg?logo=docker)](https://www.docker.com)
[![PHP](https://img.shields.io/badge/php-7.4-8892BF.svg?logo=php)](https://php.net/)
[![NGINX](https://img.shields.io/badge/nginx-1.19-009639.svg?logo=nginx)](https://www.nginx.com/)
[![MySQL](https://img.shields.io/badge/mysql-8-00758F.svg?logo=mysql)](https://www.mysql.com/)

A simple [cakephp/app 4.2](https://github.com/cakephp/app/releases/tag/4.2.1) docker setup.

| Service                   | Host:Port         | Docker Host   |
| -----------               | -----------       | -----------   |
| PHP7.4-FPM w/ Xdebug 3    | -                 | php           |
| NGINX 1.19                | localhost:8080    | web           |
| MySQL 8                   | localhost:3607    | db            |

A [Makefile](Makefile) is provided with some optional commands for your convenience.

## Installation

Fork and clone this repository then run:

```console
make init
make composer.install
```

If you prefer to do this manually, view the Makefile to see the shell commands being run.

#### Mac OSX Users
If you are using the make commands you will need `gnu-sed`, so `brew install gun-sed` and update the Makefile to
use `gsed` or you can update your system to use `gsed` permanently:
`export PATH="/usr/local/opt/gnu-sed/libex/gnubin:$PATH"`

## Usage

After install browse to [http://localhost:8080](http://localhost:8080) to see the CakePHP welcome page. 

All containers may be accessed via the following `docker exec` commands:

```console
docker exec -it <CONTAINER_NAME> sh
docker exec -it $(docker-compose ps -q php) sh
docker exec -it $(docker-compose ps -q db) sh
docker exec -it $(docker-compose ps -q web) sh
```

Makefile target commands are provided beyond this point. You may run `make up`, `make stop`, and `make restart` instead 
of the `docker-compose` equivalents.

On container restarts `.docker/*.env.development` is copied to `.docker/*.env`. These env vars may be used in 
`.docker/php/php.ini` for instance.

### PHP

See `.docker/php` for PHP INI settings. 

Shell:

```
make php.sh
make php.root.sh
```

Helper commands:

```
make composer.install
make composer.test
make composer.check
```

### MySQL

See [docker-compose.yml](docker-compose.yml) for accounts and passwords. See `.docker/mysql.env.development` for
changing host, user, db, and password.

Shell:

```console
make db.sh
```

MySQL shell (requires mysql client on your localhost):

```console
make db.mysql
```

### XDebug

Xdebug is disabled by default. To toggle:

```console
make xdebug.on
make xdebug.off
```

### PHPStorm + Xdebug

Xdebug 3's default port is `9003`.

Go to `File > Settings > Languages & Frameworks > PHP > Servers`

- Name: `localhost`
- Host: `localhost`
- Port: `8080`
- Debugger: `Xdebug`
- Use path mappings: `Enable`

Map your projects app directory to the absolute path on the docker container `/var/www/app`