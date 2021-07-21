# My Local CiviCRM Development Environment with Docker

This is my local CiviCRM development environment build with docker.
Based on the Michael McAndrew buildkit docker.

# Getting up and running 

```

git clone https://github.com/jaapjansma/my_local_development_docker.git
cd my_local_development_docker
docker-compose up -d

```

## Subsites

Sites in the directory html are set as sub sites and could be accessed by http://mytestsite.localhost:8180

## Mysql

Mysql is exposed on post 33062

### Importing mysql databases

Could be either done with dbeaver with a connection to localhost on port 33062
or with the following command

```
docker-compose exec -T mysql mysql -uroot -p --database yourdatabase < yourfile.sql
```

### Mysql Settings for your sites

**Hostname** mysql
**User** root
**Password** buildkit

## Mail

There is webmail available on http://localhost:8182 this webmail catches all outgoing mail.

## Phpmyadmin

http://localhost:8181

## Civix

Civix can then be run by going into the container

```bash

docker-compose exec -u buildkit civicrm bash
cd /var/www/html/yoursite
civix ....

```

## Drush

Drush can then be run by going into the container

```bash

docker-compose exec -u buildkit civicrm bash
cd /var/www/html/yoursite
drush ....

```

## XDebug

XDebug is installed and enabled by default.

## PHP.ini changes

You can do additional php.ini changes to docker/civicrm.ini (restart your container after making changes)
