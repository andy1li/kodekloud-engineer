# Ansible

## Level 1

### 1. Troubleshoot and Create Ansible Playbook

```bash
# thor@jump_host
vi /home/thor/ansible/inventory
> stapp02 ansible_host=172.16.238.11 ansible_user=steve ansible_ssh_pass=Am3ric@

ansible -m ping all -i ansible/inventory

vi /home/thor/ansible/playbook.yml
> - hosts: stapp02
>  gather_facts: false
>  become: true
>
>  tasks:
>    - name: Create empty file
>      file:
>        path: /tmp/file.txt
>        state: touch
```

### 2. Create Ansible Inventory for App Server Testing

```bash
# thor@jump_host
cd playbook
vi inventory
> stapp02 ansible_host=172.16.238.11 ansible_user=steve ansible_ssh_pass=Am3ric@

ansible -m ping all -i inventory
```

### 3. Configure Default SSH User for Ansible

```bash
# thor@jump_host
ansible --version
sudo vi /etc/ansible/ansible.cfg
> remote_user = jim
```

### 4. Copy Data to App Servers using Ansible

```bash
# thor@jump_host
cd ansible/
vi inventory
ansible -m ping all -i inventory

vi playbook.yml
---
- name: Copy files from jumphost to all servers
  hosts: all
  become: yes

  tasks:
  - name: Copy /usr/src/sysops/index.html file to /opt/sysops
    copy:
        src: /usr/src/security/index.html
        dest: /opt/security
```

```bash
# inventory
stapp01 ansible_host=172.16.238.10 ansible_ssh_pass=Ir0nM@n ansible_user=tony ansible_ssh_common_args='-o StrictHostKeyChecking=no'
stapp02 ansible_host=172.16.238.11 ansible_ssh_pass=Am3ric@ ansible_user=steve ansible_ssh_common_args='-o StrictHostKeyChecking=no'
stapp03 ansible_host=172.16.238.12 ansible_ssh_pass=BigGr33n ansible_user=banner ansible_ssh_common_args='-o StrictHostKeyChecking=no'
```

### 5. Create Files on App Servers using Ansible

```bash
# thor@jump_host
cd playbook/
vi inventory

vi playbook.yml
---
- name: Create blank file in the App Servers
  hosts: all
  become: yes
  tasks:
    - name: Create the file and set properties
      file:
        path: /opt/appdata.txt
        owner: "{{ ansible_user }}"
        group: "{{ ansible_user }}"
        mode: "0744"
        state: touch

ansible-playbook  -i inventory playbook.yml
```



### Test

1. The Nautilus DevOps team is running Ansible using a sudo user on the jump host, where the sudo user is configured to prompt for a password for each execution. Currently, playbooks fail when attempting tasks that require superuser privileges, such as package installation. They aim to ensure that Ansible prompts for a sudo password during playbook execution. Consequently, they have outlined the following requirements to resolve this issue:

   Please ensure the necessary adjustments are made within the Ansible configuration located at `/home/thor/ansible-t5q4/ansible-t5q4.cfg`. Avoid creating a new configuration file.

   `Note:` This is a sample Ansible configuration. If you intend to test an Ansible playbook using this configuration, you may need to explicitly set the `ANSIBLE_CONFIG` variable.

```yaml
# /home/thor/ansible-t5q4/ansible-t5q4.cfg
[privilege_escalation]
become=True
become_method=sudo
become_user=root
become_ask_pass=True
```



2. Ansible utilizes SSH connections to communicate with remote hosts. The Nautilus DevOps team intends to employ a unified Ansible manager for overseeing several remote hosts. To streamline operations, they seek to use a common remote user to connect with all remote hosts.

   Update the Ansible configuration file located at `/home/thor/ansible-t5q5/ansible-t5q5.cfg` on the jump host to set a default `remote user` as `deploy`. Please refrain from creating a new configuration file.

```yaml
# /home/thor/ansible-t5q5/ansible-t5q5.cfg
[defaults]
remote_user = deploy
```



3. There is data on `jump host` that needs to be copied on `all application servers` in `Stratos DC`. Nautilus DevOps team want to perform this task using `Ansible`. Perform the task as per details mentioned below:

   a. On `jump host` we already have inventory file `/home/thor/ansible/inventory-t2q1`.
   
   b. On `jump host` create a playbook `/home/thor/ansible/playbook-t2q1.yml` to copy `/usr/src/sysops-t2q1/index-t2q1.html` file to all application servers at location `/opt/sysops-t2q1`.
   
   `Note:` Validation will try to run the playbook using command `ansible-playbook -i inventory-t2q1 playbook-t2q1.yml` so please make sure the playbook works this way without passing any extra arguments.

```yaml
# /home/thor/ansible/playbook-t2q1.yml
---
- hosts: all
  become: true
  tasks:
    - copy:
        src: /usr/src/sysops-t2q1/index-t2q1.html
        dest: /opt/sysops-t2q1
```



4. a. On `jump host` create a playbook `/home/thor/ansible/playbook-t2q5.yml` to copy `/usr/src/sysops-t2q5/story-t2q5.txt` file from `App Server 2` at location `/opt/sysops-t2q5` on `App Server 2`.

   b. An inventory is already placed under `/home/thor/ansible/inventory-t2q5`.

   `Note:` Validation will try to run the playbook using command `ansible-playbook -i inventory-t2q5 playbook-t2q5.yml` so please make sure the playbook works this way without passing any extra arguments.

```yaml
# /home/thor/ansible/playbook-t2q5.yml
---
- hosts: stapp02
  become: true
  tasks:
    - command: cp /usr/src/sysops-t2q5/story-t2q5.txt /opt/sysops-t2q5
```



5. The Nautilus DevOps team is was doing some cleanup work on all app servers in `Stratos DC`. Instead of doing this manually they want to utilise Ansible for the same. Below are the requirements team has received.

   a. Utilise the inventory file `/home/thor/playbook/inventory-t4q4`, present on the `jump host`.

   b. Create a playbook name `/home/thor/playbook/playbook-t4q4.yml` to delete a file named `/opt/fruits-t4q4.txt` from `all App Servers`.

   `Note:` Validation will attempt to execute the playbook using the command `ansible-playbook -i inventory-t4q4 playbook-t4q4.yml`. Please ensure the playbook functions correctly with this command alone, without requiring any additional arguments.

```yaml
# /home/thor/playbook/playbook-t4q4.yml
---
- hosts: all
  become: yes

  tasks:
    - file:
        path: /opt/fruits-t4q4.txt
        state: absent
```



6. The Nautilus DevOps team is working to create some data on different app servers in using Ansible. They want to create some files/directories and have some specific requirements related to this task. Find below more details about the same:

   a. Utilise the inventory file `/home/thor/playbook/inventory-t4q3`, present on the `jump host`.

   b. Create a playbook named `/home/thor/playbook/playbook-t4q3.yml` to create a directory named `/opt/backup-t4q3` on `all App Servers`.

   `Note:` Validation will attempt to execute the playbook using the command `ansible-playbook -i inventory-t4q3 playbook-t4q3.yml`. Please ensure the playbook functions correctly with this command alone, without requiring any additional arguments.

```yaml
# /home/thor/playbook/playbook-t4q3.yml
---
- hosts: all
  become: yes

  tasks:
    - file:
        path: /opt/backup-t4q3
        state: directory
```



7. The Nautilus Application development team wanted to test some applications on app servers in `Stratos Datacenter`. They shared some pre-requisites with the DevOps team, and packages need to be installed on app servers. Since they already created some playbooks, now they wanted to make some changes in inventories.

   There is an inventory file `/home/thor/playbooks/inventory-t3q3` on `jump host`. It has some aliases named `web1`, `web2` and `web3` for three hosts respectively. Update this inventory file to add an alias called `db1` for `server4.company.com` host.

```yaml
# /home/thor/playbook/inventory-t3q3
web1 ansible_host=server1.company.com
web2 ansible_host=server2.company.com
web3 ansible_host=server3.company.com
db1  ansible_host=server4.company.com
```



8. As per the details given in the table below, you can see that, the web servers are linux based hosts and the db server is a Windows machine. Update the inventory `/home/thor/playbooks/inventory-t3q6` to add a similar entry for `server4.company.com` host.

   Find the required details from the table below.

   

   ```text
   ---------------------------------------------------------------------------
   |  Alias |        HOST         | Connection | User          | Password     | 
   ---------------------------------------------------------------------------
   |  web1  | server1.company.com |    ssh     | root          | Password123! |
   ---------------------------------------------------------------------------
   |  web2  | server2.company.com |    ssh     | root          | Password123! |
   ---------------------------------------------------------------------------
   |  web3  | server3.company.com |    ssh     | root          | Password123! |
   ---------------------------------------------------------------------------
   |  db1   | server4.company.com |    winrm   | administrator | Dbp@ss123!   |
   ---------------------------------------------------------------------------
   ```

   `Note:` For Linux based hosts, use `ansible_ssh_pass` parameter and for Windows based hosts, use `ansible_password` parameter.

   

   Updated the inventory file to add an entry for "server4.company.com" host based on the details given in the table?

```yaml
# /home/thor/playbooks/inventory-t3q6
web1 ansible_host=server1.company.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Password123!
web2 ansible_host=server2.company.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Password123!
web3 ansible_host=server3.company.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Password123!
db1 ansible_host=server4.company.com ansible_connection=winrm ansible_user=administrator ansible_password=Dbp@ss123! 
```



9. A team member completed writing a playbook, but when we attempted to execute it, an error occurred. We need someone to review the playbook, identify the issue, and fix it.

   The playbook name is `/home/thor/ansible/playbook-t1q5.yml`, make sure it executes without any error.

```yaml
# /home/thor/ansible/playbook-t1q5.yml
---
- hosts: localhost
  connection: local
  tasks:
    - name: Debug a message
      debug:
        msg: "Hello There!"
```



10. The Nautilus DevOps team recently recommended employing Ansible for configuration management and automation purposes. As they've commenced creating playbooks, a need has arisen for a basic playbook, outlined as follows.

    Create a playbook named `/home/thor/ansible/playbook-t1q2.yml` on the `jump host` and configure the playbook to run on `localhost` and execute an `echo` command echoing `Welcome!`.

```yaml
# /home/thor/ansible/playbook-t1q2.yml
---
- hosts: localhost
  connection: local
  tasks:
    - command: echo "Welcome!"
```

