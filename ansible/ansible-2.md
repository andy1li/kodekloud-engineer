# Ansible

## Level 2

### 1. Ansible Ping Module Usage

```bash
# thor@jump_host
cd ansible/
vi inventory

> stapp03 ansible_host=172.16.238.12 ansible_user=banner ansible_ssh_pass=BigGr33n

ansible stapp03 -m ping -i inventory -v
```



### 2. Ansible Install Package

```bash
# thor@jump_host
cd playbook/
vi inventory
vi playbook.yml
ansible-playbook -i inventory playbook.yml
```
```bash
# inventory
stapp01 ansible_host=172.16.238.10 ansible_ssh_pass=Ir0nM@n ansible_user=tony
stapp02 ansible_host=172.16.238.11 ansible_ssh_pass=Am3ric@ ansible_user=steve
stapp03 ansible_host=172.16.238.12 ansible_ssh_pass=BigGr33n ansible_user=banner
```

```yaml
# playbook.yml
---
- name: Install zip package on all app servers
  hosts: all
  become: yes
  tasks:
    - name: Install zip package
      yum:
        name: zip
        state: present
```



### 3. Ansible Archive Module

```bash
# thor@jump_host
cd playbook/
vi playbook.yml
ansible-playbook -i inventory playbook.yml
```

```yaml
# playbook.yml
---
- name: Create an archive and copy it
  hosts: all
  become: yes
  tasks:
    - name: Create an archive and set the ownnership
      archive:
        path: /usr/src/security/
        dest: /opt/security/official.tar.gz
        force_archive: true
        format: gz
        owner: "{{ ansible_user }}"
        group: "{{ ansible_user }}" 
```



### 4. Ansible Unarchive Module

```bash
# thor@jump_host
cd ansible/
vi playbook.yml
ansible-playbook -i inventory playbook.yml
```

```yaml
# playbook.yml
---
- name: Extract archive
  hosts: all
  become: yes
  tasks:
    - name: Extract the archive and set the owner/permissions
      unarchive:
        src: /usr/src/devops/datacenter.zip
        dest: /opt/devops/ 
        owner: "{{ ansible_user }}"
        group: "{{ ansible_user }}" 
        mode: "0777"
```



### 5. Ansible Blockinfile Module

```bash
# thor@jump_host
cd ansible/
vi playbook.yml
ansible-playbook -i inventory playbook.yml
curl stapp01
```
```yaml
# playbook.yml
---
- name: Install httpd and setup index.html
  hosts: all
  become: yes
  tasks:
     - name: Install httpd
       package:
         name: httpd
         state: present

     - name: Start service httpd, if not started
       service:
         name: httpd
         state: started

     - name: Add content block in index.html and set permissions
       blockinfile:
         path: /var/www/html/index.html
         create: yes
         owner: apache
         group: apache
         mode: "0644"
         block: |
           Welcome to XfusionCorp!
           This is Nautilus sample file, created using Ansible!
           Please do not modify this file manually!
```



### Test

1. Test

```bash
# thor@jump_host

```