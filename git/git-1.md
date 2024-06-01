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

1. A developer was in the process of creating repositories on the Gitea server. Unfortunately, one repository was mistakenly created and now needs to be deleted. Below are further details regarding this issue.

   The repository name is `story-blog-t1q1` and is located under the `sarah` user on the Gitea server.

```bash
# thor@jump_host
```

2. Recently a bare repository was created by one of the developers. Now, they were planning to add some content in this repository so this needs to be cloned somewhere so that one of the developers can start adding data in it. Below you can find more details.

   Clone the repository `/opt/story-blog-t1q11.git` under sarah user's home on storage server.

```bash
# thor@jump_host
sshpass -p 'S3cure321' ssh -o StrictHostKeyChecking=no sarah@ststor01

# sarah@ststor01
git clone /opt/story-blog-t1q11.git
```

3. A couple of new Git repositories were created recently and this is still in progress. The developers now has started adding some content under these repositories. Below are the details for this request.

   There is a file named `lion-and-mouse-t1q5.txt` which is placed under `/home/sarah/story-blog-t1q5` repository, stage the same to make it available for commit.

```bash
# sarah@ststor01
cd /home/sarah/story-blog-t1q5
git add lion-and-mouse-t1q5.txt
```

4. The Nautilus application development team was working on a git repository `/usr/src/kodekloudrepos/cluster-t2q4` present on `Storage server` in `Stratos DC`. One of the developers mistakenly created a couple of files under this repository, but now they want to clean this repository without adding/pushing any new files. Find below more details:

   Clean the `/usr/src/kodekloudrepos/cluster-t2q4` git repository without adding/pushing any new files, make sure `git status` is clean.

```bash
# sarah@ststor01
cd /usr/src/kodekloudrepos/cluster-t2q4/
git clean -f
```

5. A new repository named `/usr/src/kodekloudrepos/cluster-t2q5` was created recently and some data was added in it. Now one of the developers wanted to use this repository further to add/update some data.

   Checkout the `master` branch under repo `/usr/src/kodekloudrepos/cluster-t2q5`.

   Use below credentials to SSH into the storage server and to complete this task.

```bash
# sarah@ststor01
cd /usr/src/kodekloudrepos/cluster-t2q5
git checkout master
```
