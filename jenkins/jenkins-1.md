# Jenkins

## Level 1

### 1. Set Up Jenkins Server

```bash
# thor@jump_host
sshpass -p 'S3curePass' ssh -o StrictHostKeyChecking=no root@jenkins

# root@jenkins
yum install wget -y
sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
sudo yum upgrade
sudo yum install fontconfig java-17-openjdk -y
sudo yum install jenkins -y
sudo systemctl daemon-reload

sudo service jenkins status
sudo service jenkins start && sudo service jenkins status

cat /var/lib/jenkins/secrets/initialAdminPassword 
# 3b7aa46a21914f49b5c860ba8229b816
```



### 2. Install Jenkins Plugins

```bash
# thor@jump_host

```



### 3. Configure Jenkins User Access

```bash
# thor@jump_host

```



### 4. Organize Jenkins Jobs with Folders

```bash
# thor@jump_host

```



### 5. Configure Jenkins Job for Package Installation

```bash
# thor@jump_host

```



### Test

1. 

```bash
# thor@jump_host
```
