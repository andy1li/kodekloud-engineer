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
sshpass -p 'Am3ric@' ssh -o StrictHostKeyChecking=no steve@stapp02

# steve@stapp02
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

```



### 5. Write a Docker Compose File

```bash
# thor@jump_host

```



### Test

1. Test

```bash
# thor@jump_host

```

