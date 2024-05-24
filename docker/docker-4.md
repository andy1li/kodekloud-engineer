# Docker

## Level 4

### 1. Resolve Dockerfile Issues

```bash
# thor@jump_host
sshpass -p 'Ir0nM@n' ssh -o StrictHostKeyChecking=no tony@stapp01

# tony@stapp01
sudo vi /opt/docker/Dockerfile

> WORKDIR /usr/local/apache2/
> ... conf/httpd.conf

docker build /opt/docker/
```



### 2. Resolve Docker Compose Issues

```bash
# thor@jump_host
sshpass -p 'BigGr33n' ssh -o StrictHostKeyChecking=no banner@stapp03

# banner@stapp03
sudo curl -SL https://github.com/docker/compose/releases/download/v2.27.0/docker-compose-linux-x86_64 -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
sudo chmod 666 /var/run/docker.sock

cd /opt/docker/
sudo vi docker-compose.yml

>   services:
>			volumes:
> 		depends_on:
>			image: redis

docker-compose up -d
curl -I localhost:5000
```



### 3. Deploy an App on Docker Containers

```bash
# thor@jump_host
sshpass -p 'Am3ric@' ssh -o StrictHostKeyChecking=no steve@stapp02

# steve@stapp02
cd /opt/data/
sudo vi docker-compose.yml
docker-compose up -d
curl -I localhost:8084
```

```yml
services:
  web:
    image: php:8.1.28-apache-bullseye
    container_name: php_blog
    restart: always
    ports:
      - "8084:80"
    volumes:
      - /var/www/html:/var/www/html
    depends_on:
      - db
  db:
    image: mariadb:latest
    container_name: mysql_blog
    restart: always
    ports:
      - "3306:3306"
    volumes:
      - /var/lib/mysql:/var/lib/mysql
    environment:
      MYSQL_DATABASE: database_blog
      MARIADB_ROOT_PASSWORD: kkloud888
      MYSQL_USER: kkloud
      MYSQL_PASSWORD: kkloud666
```



### 4. Docker Node App

```bash
# thor@jump_host

```



### 5. Docker Python App

```bash
# thor@jump_host

```



### Test

1. Test

```bash
# thor@jump_host

```

