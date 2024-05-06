# Linux

## Level 1

### 1. Create a user

```bash
# thor@jump_host
sshpass -p 'Am3ric@' ssh -o StrictHostKeyChecking=no steve@stapp02

# steve@stapp02
sudo useradd -u 1426 -d /var/www/james -m james
id james
```

> - `-u 1426`: Specifies the UID (User ID) for the user "james" as 1426.
> - `-d /var/www/james`: Sets the home directory for the user "james" to `/var/www/james`.
> - `-m`: Creates the home directory if it doesn't exist.




### 2. Create a group

```bash
# thor@jump_host
sshpass -p 'Ir0nM@n' ssh -o StrictHostKeyChecking=no tony@stapp01

# tony@stapp01
sudo groupadd nautilus_admin_users
getent group nautilus_admin_users

sudo adduser -g nautilus_admin_users john
id john

# repeat 2 times
sshpass -p 'Am3ric@' ssh -o StrictHostKeyChecking=no steve@stapp02
sshpass -p 'BigGr33n' ssh -o StrictHostKeyChecking=no banner@stapp03
```



### 3. Create a Linux User with non-interactive shell

```bash
# thor@jump_host
sshpass -p 'BigGr33n' ssh -o StrictHostKeyChecking=no banner@stapp03

# banner@stapp03
sudo adduser -s /sbin/nologin jim
id jim
```



### 4. Linux User Without Home

```bash
# thor@jump_host
sshpass -p 'Am3ric@' ssh -o StrictHostKeyChecking=no steve@stapp02

# steve@stapp02
sudo adduser -M rose
id rose
```



### 5. Linux User Expiry

```bash
# thor@jump_host
sshpass -p 'BigGr33n' ssh -o StrictHostKeyChecking=no banner@stapp03

# banner@stapp03
sudo adduser -e 2024-03-28 rose
chage -l rose
```



### 6. Linux User Files

```bash
# thor@jump_host
sshpass -p 'Ir0nM@n' ssh -o StrictHostKeyChecking=no tony@stapp01

# tony@stapp01
find /home/usersdata -type f -user siva -exec cp --parents {} /ecommerce \;
```



### 7. Disable Root Login

```bash
# thor@jump_host
sshpass -p 'Ir0nM@n' ssh -o StrictHostKeyChecking=no tony@stapp01

# tony@stapp01
sudo vi /etc/ssh/sshd_config
`PermitRootLogin no`

sudo systemctl reload sshd

# repeat 2 times
sshpass -p 'Am3ric@' ssh -o StrictHostKeyChecking=no steve@stapp02
sshpass -p 'BigGr33n' ssh -o StrictHostKeyChecking=no banner@stapp03
```



### 8. Linux Archives

```bash
# thor@jump_host

```

