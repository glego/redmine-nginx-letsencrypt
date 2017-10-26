# Docker Compose for Redmine with NGINX Reverse Proxy and Let's Encrypt - Free SSL/TLS Certificate

`docker-compose.yml` sets up a [docker-gen][docker-gen-github], [letsencrypt-proxy-companion][docker-letsencrypt-github] and [redmine][redmine-dockerhub] in a single go. It will open the ports 80 and 433 on the host and automatically retrieve an certificate from [Let's Encrypt][letsencrypt]. Make sure to change the `VIRTUAL_HOST`, `LETSENCRYPT_HOST` and `LETSENCRYPT_EMAIL` before starting the Docker containers.

## How to use

1. Clone the repository

```shell
# Clone the repository
git clone https://github.com/glego/redmine-nginx-letsencrypt

# Change into the redmine-nginx-letsencrypt directory
cd ./redmine-nginx-letsencrypt
```

2. Edit the file `docker-compose.yml` and change the `VIRTUAL_HOST`, `LETSENCRYPT_HOST` and `LETSENCRYPT_EMAIL`

3. Start the containers by using docker-compose.

```shell
# Start Docker Containers in the foreground (attached mode)
docker-compose up

# Start Docker Containers in the background (deattached mode)
docker-compose -d up
```

## Prerequisites

Make sure that the following packages are installed and up-to-date for your operation system.

- [Docker][docker-installation]
- [Docker-Compose][docker-compose]
- (sub)domains and a publicly available web server

## Troubleshooting

- View the docker compose logs

```shell
docker-compose logs
```

- View the NGINX configuration file

```shell
docker exec -ti nginx cat /etc/nginx/conf.d/default.conf
```

- View the ports used by the containers

```
$ docker ps
CONTAINER ID        IMAGE                                    COMMAND                  CREATED             STATUS              PORTS                                      NAMES
1e064a79d46f        jwilder/docker-gen                       "/usr/local/bin/do..."   11 minutes ago      Up 11 minutes                                                  nginx-gen
3a679652ca95        jrcs/letsencrypt-nginx-proxy-companion   "/bin/bash /app/en..."   11 minutes ago      Up 11 minutes                                                  letsencrypt-nginx-proxy-companion
23beda8a0018        nginx                                    "nginx -g 'daemon ..."   11 minutes ago      Up 11 minutes       0.0.0.0:80->80/tcp, 0.0.0.0:443->443/tcp   nginx
a1e6a59e1864        redmine                                  "/docker-entrypoin..."   11 minutes ago      Up 11 minutes       3000/tcp                                   redminenginxletsencrypt_redmine_1
```

> Under the ports column, container nginx has `0.0.0.0:80->80/tcp, 0.0.0.0:443->443/tcp`, which means it's bound to the host (visible from the internet). While for redmine `3000/tcp` is only visible for the other containers.

## Notes

- I've tested the docker compose file on the following system:

```shell
$ lsb_release -a
No LSB modules are available.
Distributor ID: Debian
Description:    Debian GNU/Linux 9.2 (stretch)
Release:        9.2
Codename:       stretch

$ docker --version
Docker version 17.09.0-ce, build afdb6d4

$ docker-compose --version
docker-compose version 1.16.1, build 6d1ac21
```

- Some cheap VPS that supports Docker
  - [https://www.vultr.com/](https://www.vultr.com/?ref=7244423)
  - https://www.vps.ag/
  - https://www.ssdnodes.com/


## References

* https://github.com/jwilder/docker-gen
* https://github.com/JrCs/docker-letsencrypt-nginx-proxy-companion
* https://hub.docker.com/_/redmine/

[docker-gen-github]:https://github.com/jwilder/docker-gen
[docker-letsencrypt-github]:https://github.com/JrCs/docker-letsencrypt-nginx-proxy-companion
[redmine-dockerhub]:https://hub.docker.com/_/redmine/
[letsencrypt]:https://letsencrypt.org/
[docker-installation]:https://docs.docker.com/engine/installation/
[docker-compose]:https://docs.docker.com/compose/install/
