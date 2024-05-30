# Git

## Level 1

### 1. Git Install and Create Bare Repository

```bash
# thor@jump_host
sshpass -p 'Bl@kW' ssh -o StrictHostKeyChecking=no natasha@ststor01

# natasha@ststor01
yum install -y git
cd /opt/
sudo git init --bare news.git
```



### 2. Git Clone Repositories

```bash
# thor@jump_host
sshpass -p 'Bl@kW' ssh -o StrictHostKeyChecking=no natasha@ststor01

# natasha@ststor01
cd /usr/src/kodekloudrepos/
sudo git clone /opt/cluster.git/
```



### 3. Git Fork a Repository

```bash
# thor@jump_host
```



### 4. Git Repository Update

```bash
# thor@jump_host
scp /tmp/index.html natasha@ststor01:/tmp/
sshpass -p 'Bl@kW' ssh -o StrictHostKeyChecking=no natasha@ststor01

# natasha@ststor01
sudo cp /tmp/index.html /usr/src/kodekloudrepos/blog/index.html
cd /usr/src/kodekloudrepos/blog/
git config --global --add safe.directory /usr/src/kodekloudrepos/blog
sudo git add index.html
sudo git commit -m "add index.html"
sudo git push -u origin master
```



### 5. Git Delete Branches

```bash
# thor@jump_host
sshpass -p 'Bl@kW' ssh -o StrictHostKeyChecking=no natasha@ststor01

# natasha@ststor01
cd /usr/src/kodekloudrepos/media
sudo git branch -a
sudo git checkout master
sudo git branch -d xfusioncorp_media
```



### Test

1. Test

```bash
# thor@jump_host

```

