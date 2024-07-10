# Ansible

## Level 4

### 1. Ansible Facts Gathering

```bash
# thor@jump_host
cd playbooks/
vi index.yml
ansible-playbook -i inventory index.yml
curl stapp01
```

```yaml
# index.yml
---
- name: Create the sample html page and install Apache
  hosts: all
  gather_facts: true
  become: yes
  become_method: sudo
  
  tasks:
    - name: Use blockinfile to create facts.txt
      blockinfile:
        create: yes
        path: /root/facts.txt
        block: |
          Ansible managed node architecture is {{ ansible_architecture }}

    - name: Install Apache
      package:
        name: httpd
        state: present

    - name: Copy facts.txt to index.html
      shell: |
        cp /root/facts.txt /var/www/html/index.html

    - name: Ensure httpd is running
      service:
        name: httpd
        state: started
```



### 2. Ansible Create Users and Groups

```bash
# thor@jump_host
cd playbooks/
vi add_users.yml
vi ansible.cfg
ansible-playbook add_users.yml
```

```yaml
# add_users.yml
---
- name: Create Users and Groups on App Server 1
  hosts: stapp01
  become: yes
  tasks:
  
  - name: Create the Admin Group 
    group:
        name: admins 
        state: present
        
  - name: Create the Dev Group
    group:
        name: developers 
        state: present
        
  - name: Create the Users for Admin Group
    user:
        name: "{{ item }}"
        password: "{{ 'dCV3szSGNA' | password_hash ('sha512') }}"
        groups: admins, wheel 
        state: present 
    loop:
    - rob
    - david
    - joy
    
  - name: Create the Users for Dev Group
    user:
        name: "{{ item }}"
        password: "{{ '8FmzjvFU6S' | password_hash ('sha512') }}"
        groups: developers 
        home: "/var/www/{{ item }}"
        state: present 
    loop:
    - tim
    - ray 
    - jim
    - mark
```
```
# ansible.cfg
[defaults]
host_key_checking = False 
inventory = ~/playbooks/inventory
vault_password_file = ~/playbooks/secrets/vault.txt
```



### 3. Managing Jinja2 Templates Using Ansible

```bash
# thor@jump_host
cd ansible/
vi playbook.yml
sudo vi role/httpd/templates/index.html.j2
sudo vi role/httpd/tasks/main.yml
ansible-playbook -i inventory playbook.yml
curl stapp02
```
```yaml
# playbook.yml
---
- hosts: stapp02
  become: yes
  become_user: root
  roles:
    - role/httpd
```
```jinja2
{# /home/thor/ansible/role/httpd/templates/index.html.j2 #}
This file was created using Ansible on {{ inventory_hostname }}
```
```yaml
# /home/thor/ansible/role/httpd/tasks/main.yml
---
- name: install the latest version of HTTPD
  yum:
    name: httpd
    state: latest

- name: Start service httpd
  service:
    name: httpd
    state: started

- name: Add index.html
  template: 
    src: /home/thor/ansible/role/httpd/templates/index.html.j2
    dest: /var/www/html/index.html
    mode: '0744'
    owner: "{{ ansible_user }}"
    group: "{{ ansible_user }}"
  become: yes
  become_user: root
```




### 4. Ansible Setup Httpd and PHP

```bash
# thor@jump_host
cd playbooks
vi httpd.yml
ansible-playbook -i inventory httpd.yml
curl -I stapp01
```

```yaml
# httpd.yml
---
- name: Setup HTTPD and PHP on App Server 1
  hosts: stapp01
  become: yes
  tasks:
  
    - name: Install latest version of httpd and php
      package:
        name:
          - httpd
          - php
        state: latest
        
    - name: Replace default DocumentRoot in httpd.conf
      replace:
        path: /etc/httpd/conf/httpd.conf
        regexp: DocumentRoot \"\/var\/www\/html\"
        replace: DocumentRoot "/var/www/html/myroot"
        
    - name: If non-existent, create the new DocumentRoot directory 
      file:
        path: /var/www/html/myroot
        state: directory
        owner: apache
        group: apache
        
    - name: Generate phpinfo.php
      template:
        src: /home/thor/playbooks/templates/phpinfo.php.j2
        dest: /var/www/html/myroot/phpinfo.php
        owner: apache
        group: apache
        
    - name: Start and enable service httpd
      service:
        name: httpd
        state: started
        enabled: yes
```



### 5. Using Ansible Conditionals

```bash
# thor@jump_host
cd ansible/
vi playbook.yml
ansible-playbook -i inventory playbook.yml
```

```yaml
# playbook.yml
---
- name: Copy files to App Servers
  hosts: all
  become: yes
  tasks:
  
    - name: Copy blog.txt to stapp01
      copy:
        src: /usr/src/sysops/blog.txt
        dest: /opt/sysops/
        owner: tony
        group: tony
        mode: "0777"
      when: inventory_hostname == "stapp01"
      
    - name: Copy story.txt to stapp02
      copy:
        src: /usr/src/sysops/story.txt
        dest: /opt/sysops/
        owner: steve
        group: steve
        mode: "0777"
      when: inventory_hostname == "stapp02"
      
    - name: Copy media.txt to stapp03
      copy:
        src: /usr/src/sysops/media.txt
        dest: /opt/sysops/
        owner: banner
        group: banner
        mode: "0777"
      when: inventory_hostname == "stapp03"
```



### Test

1. Test

```bash
# thor@jump_host

```