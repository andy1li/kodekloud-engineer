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

```



### 4. Docker EXEC Operations

```bash
# thor@jump_host

```



### 5. Write a Docker File

```bash
# thor@jump_host

```



### Test

1. Test

```bash
# thor@jump_host

```

