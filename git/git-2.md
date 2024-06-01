# Git

## Level 2

### 1. Git Install and Create Repository

```bash
# thor@jump_host
sshpass -p 'Bl@kW' ssh -o StrictHostKeyChecking=no natasha@ststor01

# natasha@ststor01
sudo yum install git -y
rpm -qa 'git'
cd /opt/
sudo git init news.git
```



### 2. Git Create Branches

```bash
# thor@jump_host
sshpass -p 'Bl@kW' ssh -o StrictHostKeyChecking=no natasha@ststor01

# natasha@ststor01
cd /usr/src/kodekloudrepos/beta
sudo git checkout master
sudo git checkout -b xfusioncorp_beta
sudo git branch -a
```



### 3. Git Merge Branches

```bash
# thor@jump_host
sshpass -p 'Bl@kW' ssh -o StrictHostKeyChecking=no natasha@ststor01

# natasha@ststor01
cd /usr/src/kodekloudrepos/media/
sudo git branch -a
sudo git checkout -b nautilus

sudo cp /tmp/index.html .
sudo git add index.html
sudo git commit -m "add index.html"

sudo git checkout master
sudo git merge nautilus

sudo git push -u origin master
sudo git push -u origin nautilus
```



### 4. Git Manage Remotes

```bash
# thor@jump_host
sshpass -p 'Bl@kW' ssh -o StrictHostKeyChecking=no natasha@ststor01

# natasha@ststor01
cd /usr/src/kodekloudrepos/official/
sudo git remote add dev_official /opt/xfusioncorp_official.git

sudo cp /tmp/index.html .
sudo git add index.html
sudo git commit -m "add index.html"

sudo git push -u dev_official master
```



### 5. Git Revert Some Changes

```bash
# thor@jump_host
sshpass -p 'Bl@kW' ssh -o StrictHostKeyChecking=no natasha@ststor01

# natasha@ststor01
cd /usr/src/kodekloudrepos/games/
sudo git log
sudo git revert HEAD
> revert games
```



### Test

1. Test

```bash
# thor@jump_host

```

