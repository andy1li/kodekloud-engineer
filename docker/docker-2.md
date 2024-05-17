# Docker

## Level 2

### 1. Pull Docker Image

```bash
# thor@jump_host
sshpass -p 'Ir0nM@n' ssh -o StrictHostKeyChecking=no tony@stapp01

# tony@stapp01
docker pull busybox:musl
docker tag busybox:musl busybox:media
docker images
```



### 2. Docker Update Permissions

```bash
# thor@jump_host
sshpass -p 'BigGr33n' ssh -o StrictHostKeyChecking=no banner@stapp03

# banner@stapp03
id jim
getent group docker
sudo usermod -aG docker jim
getent group docker
sudo su - jim

# jim@stapp03
docker ps -a
```



### 3. Create a Docker Image From Container 

```bash
# thor@jump_host
sshpass -p 'Am3ric@' ssh -o StrictHostKeyChecking=no steve@stapp02

# steve@stapp02
docker ps -a
docker commit ubuntu_latest apps:devops 
docker images
```



### 4. Docker EXEC Operations

```bash
# thor@jump_host
sshpass -p 'BigGr33n' ssh -o StrictHostKeyChecking=no banner@stapp03

# banner@stapp03
docker ps -a
docker exec -it kkloud /bin/bash
apt install apache2 -y

cd /etc/apache2
sed -i 's/Listen 80/Listen 6100/g' ports.conf

service apache2 start
service apache2 status
curl -I localhost:6100
```



### 5. Write a Docker File

```bash
# thor@jump_host
sshpass -p 'Am3ric@' ssh -o StrictHostKeyChecking=no steve@stapp02

# steve@stapp02
sudo vi /opt/docker/Dockerfile

> FROM ubuntu
> RUN apt-get update && apt-get install -y apache2
> RUN sed -i 's/Listen 80/Listen 6200/g' /etc/apache2/ports.conf
> EXPOSE 6200
> CMD ["apachectl", "-D", "FOREGROUND"]

docker build -t ubuntu-apache2 /opt/docker/
docker run -d -p 6200:6200 ubuntu-apache2
curl -I localhost:6200
```



### Test

1. Test

```bash
# thor@jump_host

```

