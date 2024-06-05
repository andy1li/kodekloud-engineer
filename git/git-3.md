# Git

## Level 3

### 1. Git Cherry Pick

```bash
# thor@jump_host
sshpass -p 'Bl@kW' ssh -o StrictHostKeyChecking=no natasha@ststor01

# natasha@ststor01
cd /usr/src/kodekloudrepos/demo/
sudo git checkout master
sudo git log feature
sudo git cherry-pick eb279d787817375155947ccdaaeda9c145dcb1c6
sudo git push
```



### 2. Manage Git Pull Requests

```bash
# thor@jump_host
```



### 3. Git hard reset

```bash
# thor@jump_host
sshpass -p 'Bl@kW' ssh -o StrictHostKeyChecking=no natasha@ststor01

# natasha@ststor01
cd /usr/src/kodekloudrepos/apps/
sudo git log
sudo git reset --hard 8dba420aa862765b43e654d196fb18b72be41242
sudo git push -f
```



### 4. Git Clean

```bash
# thor@jump_host
sshpass -p 'Bl@kW' ssh -o StrictHostKeyChecking=no natasha@ststor01

# natasha@ststor01
cd /usr/src/kodekloudrepos/demo/
sudo git clean -f
sudo git status
```



### 5. Git Stash

```bash
# thor@jump_host
sshpass -p 'Bl@kW' ssh -o StrictHostKeyChecking=no natasha@ststor01

# natasha@ststor01
cd /usr/src/kodekloudrepos/media/
sudo git stash list
sudo git stash pop
sudo git add welcome.txt
sudo git commit -m "add welcome.txt"
sudo git push
```



### Test

1. Test

```bash
# thor@jump_host

```

