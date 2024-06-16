# Git

## Level 4

### 1. Git Rebase

```bash
# thor@jump_host
sshpass -p 'Bl@kW' ssh -o StrictHostKeyChecking=no natasha@ststor01

# natasha@ststor01
cd /usr/src/kodekloudrepos/ecommerce/
sudo git rebase master

sudo git pull origin feature --rebase
sudo git push origin feature
```



### 2. Manage Git Repositories

```bash
# thor@jump_host
sshpass -p 'Max_pass123' ssh -o StrictHostKeyChecking=no max@ststor01

# max@ststor01
git clone http://git.stratos.xfusioncorp.com/max/story_official.git
cp -a /usr/dba/. story_official/
cd story_official/
git add .
git commit -m "add stories"
git push --set-upstream origin master

git checkout -b max_demo
vi story-index-max.txt
> . The Lion and the Mouse
git add .
git commit -m "typo fixed for Mooose"
git push --set-upstream origin max_demo

```



### 3. Resolve Git Merge Conflicts

```bash
# thor@jump_host
sshpass -p 'Max_pass123' ssh -o StrictHostKeyChecking=no max@ststor01

# max@ststor01
cd story-blog/
git push
git pull

vi story-index.txt
git add story-index.txt
git commit -m "resolve merge conflict"
git push
```



### 4. Git Hook

```bash
# thor@jump_host
sshpass -p 'Bl@kW' ssh -o StrictHostKeyChecking=no natasha@ststor01

# natasha@ststor01
cd /opt/ecommerce.git/hooks/
sudo vi post-update
sudo chmod +x post-update

cd /usr/src/kodekloudrepos/ecommerce
sudo git checkout master
sudo git merge feature
sudo git fetch --tags
sudo git tag
```

```bash
#!/bin/bash

current_branch=$(git symbolic-ref HEAD)

if [[ "$current_branch" == "refs/heads/master" ]]; then
  current_date=$(date +%Y-%m-%d)
  tag_name="release-$current_date"
  git tag $tag_name
fi
```



### 5. Git Setup from Scratch

```bash
# thor@jump_host
sshpass -p 'Bl@kW' ssh -o StrictHostKeyChecking=no natasha@ststor01

# natasha@ststor01
sudo yum install git -y
git config --global --add user.name natasha
git config --global --add user.email natasha@stratos.xfusioncorp.com

sudo git init --bare /opt/ecommerce.git
sudo cp /tmp/update /opt/ecommerce.git/hooks/

cd /usr/src/kodekloudrepos/ 
sudo git clone /opt/ecommerce.git/

cd ecommerce/
sudo git checkout -b xfusioncorp_ecommerce

sudo cp /tmp/readme.md .
sudo git add readme.md
sudo git commit -m "add readme.md"
sudo git push origin xfusioncorp_ecommerce

sudo git checkout -b master
sudo git push
```



### Test

1. Test

```bash
# thor@jump_host

```

