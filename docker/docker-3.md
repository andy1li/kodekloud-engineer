# Docker

## Level 3

### 1. Create a Docker Network

```bash
# thor@jump_host
sshpass -p 'Am3ric@' ssh -o StrictHostKeyChecking=no steve@stapp02

# steve@stapp02
docker network ls

docker network create beta  \
  --driver bridge \
  --subnet 172.28.0.0/24 \
  --ip-range 172.28.0.2/24 

docker network ls
```



### 2. Docker Volumes Mapping

```bash
# thor@jump_host
sshpass -p 'BigGr33n' ssh -o StrictHostKeyChecking=no banner@stapp03

# banner@stapp03
docker run -d \
	--name demo \
	-v /opt/security:/tmp \
	nginx
sudo cp /tmp/sample.txt /opt/security
```



### 3. Docker Ports Mapping

```bash
# thor@jump_host
sshpass -p 'Am3ric@' ssh -o StrictHostKeyChecking=no steve@stapp02

# steve@stapp02
docker run -d \
	--name media \
	-p 3001:80 \
	nginx:alpine-perl
curl -I localhost:3001
```



### 4. Save, Load and Transfer Docker Image

```bash
# thor@jump_host
sshpass -p 'Ir0nM@n' ssh -o StrictHostKeyChecking=no tony@stapp01

# tony@stapp01
docker save apps:datacenter > apps.tar
scp apps.tar banner@stapp03:/home/banner
sshpass -p 'BigGr33n' ssh -o StrictHostKeyChecking=no banner@stapp03

# banner@stapp03
docker load < apps.tar
docker images
```



### 5. Write a Docker Compose File

```bash
# thor@jump_host
sshpass -p 'Am3ric@' ssh -o StrictHostKeyChecking=no steve@stapp02

# steve@stapp02
sudo curl -SL https://github.com/docker/compose/releases/download/v2.27.0/docker-compose-linux-x86_64 -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose

cd /opt/docker/
sudo vi docker-compose.yml

>  services:
>  httpd-container:
>    image: httpd:latest
>    container_name: httpd
>    ports:
>      - "8084:80"
>    volumes:
>      - /opt/devops:/usr/local/apache2/htdocs

docker-compose up -d
```



### Test

1. Test

```bash
# thor@jump_host

```

