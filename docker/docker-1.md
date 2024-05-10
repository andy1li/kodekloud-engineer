# Docker

## Level 1

### 1. Install Docker Package

```bash
# thor@jump_host
sshpass -p 'BigGr33n' ssh -o StrictHostKeyChecking=no banner@stapp03

# banner@stapp03
# https://docs.docker.com/engine/install/centos/
# https://docs.docker.com/compose/install/linux/
```



### 2. Run a Docker Container

```bash
# thor@jump_host
sshpass -p 'BigGr33n' ssh -o StrictHostKeyChecking=no banner@stapp03

# banner@stapp03
docker run -d --name nginx_3 -p 80:80 nginx:alpine
curl -I localhost
```



### 3. Docker Delete Container

```bash
# thor@jump_host
sshpass -p 'Ir0nM@n' ssh -o StrictHostKeyChecking=no tony@stapp01

# tony@stapp01
docker ps -a
docker stop kke-container
docker rm kke-container
docker ps -a
```



### 4. Docker Copy Operations

```bash
# thor@jump_host
sshpass -p 'BigGr33n' ssh -o StrictHostKeyChecking=no banner@stapp03

# banner@stapp03
docker ps -a
docker cp /tmp/nautilus.txt.gpg ubuntu_latest:/opt/
docker exec ubuntu_latest ls -lah /opt
```



### 5. Docker Container Issue

```bash
# thor@jump_host
sshpass -p 'Ir0nM@n' ssh -o StrictHostKeyChecking=no tony@stapp01

# tony@stapp01
docker ps -a
docker logs nautilus
docker start nautilus
curl localhost:8080
```



### Test

1. The Nautilus DevOps team is testing some applications deployment on some of the application servers. They need to deploy a nginx container on `Application Server 2`. Please complete the task as per details given below:

On `Application Server 2` create a container named `nginx_2` using image `nginx` with `alpine` tag and make sure container is in `running` state.

```bash
# thor@jump_host
sshpass -p 'Am3ric@' ssh -o StrictHostKeyChecking=no steve@stapp02

# steve@stapp02
docker run -d --name nginx_2 -p 80:80 nginx:alpine
curl -I localhost
```

2. The Nautilus team wants to create a debug container on `Application Server 2`. However, they had some specific requirements related to the CMD. Please complete the task as per details given below:

   a. On `Application Server 2` create a container named `debug_2` using image `ubuntu/apache2:latest`.

   b. Overwrite the default CMD with command `sleep 1000`.

   c. Make sure the container is in `running` state.

```bash
# steve@stapp02
docker run -d --name debug_2 ubuntu/apache2:latest sleep 1000
```

3. The Nautilus DevOps team has some confidential data present on `App Server 2` in `Stratos Datacenter`. There is a container `ubuntu_latest` running on the same server. We received a request to copy some of the data from the docker host to the container. Below are more details about the task:

   On `App Server 2` in `Stratos Datacenter` copy an encrypted file `/tmp/nautilus.txt.gpg` from docker host to `ubuntu_latest` container (running on same server) in `/opt/` location (create this location if doesn't exit). Please do not try to modify this file in any way.

```bash
# steve@stapp02
docker cp /tmp/nautilus.txt.gpg ubuntu_latest:/opt/
docker exec ubuntu_latest ls -lah /opt
```

4. We received a request to copy some of the data from one of the docker containers to the docker host. The container is running on `App Server 2` in `Stratos Datacenter`. Below are more details about the task:

   On `App Server 2` in `Stratos Datacenter` copy an encrypted file `/tmp/test.txt.gpg` from `development_3` docker container to the docker host in `/tmp` location. Please do not try to modify this file in any way.
```bash
# steve@stapp02
docker cp development_3:/tmp/test.txt.gpg /tmp
ls -lah /tmp
```

5. The Nautilus DevOps team was testing a custom container on `Application Server 2` in `Stratos DC`. They were able to configure it as per their requirements, now they wanted to create an image from this container.

   a. The name of the container is `alpine_nautilus`.

   b. Create an image `alpine:nautilus` from this container.
```bash
# steve@stapp02
docker commit alpine_nautilus alpine:nautilus
docker images
```

6. We need some docker images on `Application Server 2`, these images will be used to create some containers later. Pull below mentioned images on `Application Server 2` in `Stratos DC`.

   a. redis:alpine

   b. memcached:alpine
```bash
# steve@stapp02
docker pull redis:alpine
docker pull memcached:alpine
docker images
```

7. The Nautilus DevOps team is planning to do some cleanup on `App Server 2` in `Stratos Datacenter`, some old and unused docker networks need to be deleted. Find below more details:

   Delete a docker network named `php-network` from `App Server 2` in `Stratos Datacenter`.
```bash
# steve@stapp02
docker network ls
docker network rm php-network
```

8. The Nautilus DevOps team is planning to setup/create some docker containers on `App Server 2` in `Stratos Datacenter`, some prerequisites are needs to be done on this server. Find below more details:

   Create a new network named `mysql-network` using the `bridge` driver. Allocate subnet `182.18.0.1/24`, configure Gateway `182.18.0.1`.
```bash
# steve@stapp02
docker network create --driver bridge --subnet 182.18.0.1/24 --gateway 182.18.0.1 mysql-network
```

9. There is a static website running within a container named `nautilus`, this container is running on `App Server 2`. Suddenly, we started facing some issues with the static website on `App Server 2`. Look into the issue to fix the same, you can find more details below:

   a. Container's volume `/usr/local/apache2/htdocs` is mapped with the host volume `/var/www/html`.

   b. The website should run on host port `8080` on `App Server 2` i.e command `curl http://localhost:8080/` should work on `App Server 2`.
```bash
# steve@stapp02
docker logs nautilus
docker start nautilus
curl localhost:8080
```
