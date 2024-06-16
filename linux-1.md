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
sshpass -p 'Bl@kW' ssh -o StrictHostKeyChecking=no natasha@ststor01

# natasha@ststor01
tar -cvzf kirsty.tar.gz /data/kirsty
sudo mv kirsty.tar.gz /home
```



### 9. Linux File Permissions

```bash
# thor@jump_host
sshpass -p 'Am3ric@' ssh -o StrictHostKeyChecking=no steve@stapp02

# steve@stapp02
ls -lh /tmp/xfusioncorp.sh
sudo chmod +rx /tmp/xfusioncorp.sh
/tmp/xfusioncorp.sh
```



### 10. Linux Access Control List

```bash
# thor@jump_host
sshpass -p 'Ir0nM@n' ssh -o StrictHostKeyChecking=no tony@stapp01

# tony@stapp01
getfacl /etc/hostname
sudo setfacl -m u:ammar:-,jerome:r /etc/hostname
getfacl /etc/hostname
```



### 11. Linux String Substitute

```bash
# thor@jump_host
sshpass -p 'H@wk3y3' ssh -o StrictHostKeyChecking=no clint@stbkp01

# clint@stbkp01
sudo tail /root/nautilus.xml
sudo grep -e About /root/nautilus.xml | wc -l
sudo sed -i 's#About#Architecture#g' /root/nautilus.xml
sudo grep -e Architecture /root/nautilus.xml | wc -l
```



### 12. Secure Data Transfer

```bash
# thor@jump_host
scp /tmp/nautilus.txt.gpg steve@stapp02:/home/nfsdata/
```



### 13. Restrict Cron Access

```bash
# thor@jump_host
sshpass -p 'Am3ric@' ssh -o StrictHostKeyChecking=no steve@stapp02

# steve@stapp02
sudo su

# root@stapp02
echo "jim" >> /etc/cron.allow
echo "rod" >> /etc/cron.deny
exit

sudo su jim
# jim@stapp02
crontab -e
exit

sudo su rod
# rod@stapp02
crontab -e
```



### 14. Default GUI Boot Configuration

```bash
# thor@jump_host
sshpass -p 'Ir0nM@n' ssh -o StrictHostKeyChecking=no tony@stapp01

# tony@stapp01
systemctl list-units | grep -i target

systemctl get-default
sudo systemctl set-default graphical.target

sudo systemctl start graphical.target
sudo systemctl status graphical.target

# repeat 2 times
sshpass -p 'Am3ric@' ssh -o StrictHostKeyChecking=no steve@stapp02
sshpass -p 'BigGr33n' ssh -o StrictHostKeyChecking=no banner@stapp03
```



### 15. Timezone Alignment

```bash
# thor@jump_host
sshpass -p 'Ir0nM@n' ssh -o StrictHostKeyChecking=no tony@stapp01

# tony@stapp01
sudo timedatectl set-timezone Pacific/Fiji
timedatectl

# repeat 2 times
sshpass -p 'Am3ric@' ssh -o StrictHostKeyChecking=no steve@stapp02
sshpass -p 'BigGr33n' ssh -o StrictHostKeyChecking=no banner@stapp03
```



### 16. NTP Configuration

```bash
# thor@jump_host
sshpass -p 'Am3ric@' ssh -o StrictHostKeyChecking=no steve@stapp02

# steve@stapp02
sudo yum install -y ntp
sudo vi /etc/ntp.conf
> server 1.my.pool.ntp.org
```



### 17. Firewall Configuration

```bash
# thor@jump_host

```



### 18. Process Limit Adjustment

```bash
# thor@jump_host

```



### 19. SElinux Installation and Configuration

```bash
# thor@jump_host

```

