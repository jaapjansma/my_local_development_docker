# My Local CiviCRM Development Environment with Docker

This is my local CiviCRM development environment build with docker.
Based on the Michael McAndrew buildkit docker.

## Subsites

Sites in the directory html are set as sub sites and could be accessed by http://mytestsite.localhost:8180

## Mysql

Mysql is exposed on post 33062

## Mail

There is webmail available on http://localhost:8182 this webmail catches all outgoing mail.

## Phpmyadmin

http://localhost:8181

## Civix

Civix can ben run by going into the container

```bash

docker-compose exec -u buildkit civicrm bash
cd /var/www/html/yoursite
civix ....

```