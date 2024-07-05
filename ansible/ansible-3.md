# Ansible

## Level 3

### 1. Creating Soft Links Using Ansible

```bash
# thor@jump_host
cd ansible/
vi playbook.yml
ansible-playbook -i inventory playbook.yml
ansible all -a "ls -l /opt/sysops" -i inventory
ansible all -a "ls -l /var/www/html" -i inventory
```

```yaml
# playbook.yml
---
- hosts: stapp01
  become: true 
  tasks:
    - name: Create empty file 
      file:
        path: /opt/sysops/blog.txt
        state: touch
        owner: tony 
        group: tony
        
- hosts: stapp02
  become: true 
  tasks:
    - name: Create empty file 
      file:
        path: /opt/sysops/story.txt
        state: touch
        owner: steve 
        group: steve
    
- hosts: stapp03
  become: true 
  tasks:
    - name: Create empty file 
      file:
        path: /opt/sysops/media.txt
        state: touch
        owner: banner 
        group: banner
        
- hosts: all
  become: true 
  tasks:
    - name: Create symbolic link 
      file:
        state: link
        src: /opt/sysops
        dest: /var/www/html
```



### 2. Managing ACLs Using Ansible

```bash
# thor@jump_host
cd ansible/
vi playbook.yml
ansible-playbook -i inventory playbook.yml
ansible all -a "ls -l /opt/finance/" -i inventory
```

```yaml
# playbook.yml
---
- name: Create file and set ACL in Host 1
  hosts: stapp01
  become: yes
  tasks:
    - name: Create the blog.txt on stapp01
      file:
        path: /opt/finance/blog.txt
        state: touch
    - name: Set ACL for blog.txt
      acl:
        path: /opt/finance/blog.txt
        entity: tony
        etype: group
        permissions: r
        state: present
        
- name: Create file and set ACL in Host 2
  hosts: stapp02
  become: yes
  tasks:
    - name: Create the story.txt on stapp02
      file:
        path: /opt/finance/story.txt
        state: touch
    - name: Set ACL for blog.txt
      acl:
        path: /opt/finance/story.txt
        entity: steve
        etype: user
        permissions: rw
        state: present

- name: Create file and set ACL in Host 3
  hosts: stapp03
  become: yes
  tasks:
    - name: Create the media.txt on stapp02
      file:
        path: /opt/finance/media.txt
        state: touch
    - name: Set ACL for blog.txt
      acl:
        path: /opt/finance/media.txt
        entity: banner
        etype: group
        permissions: rw
        state: present
```



### 3. Ansible Manage Services

```bash
# thor@jump_host
cd ansible/
vi playbook.yml
ansible-playbook -i inventory playbook.yml
```

```yaml
# playbook.yml
---
- name: Install and configure httpd
  hosts: all
  become: true
  tasks:
    - name: Install httpd package
      yum:
        name: httpd
        state: present

    - name: Start httpd service
      systemd:
        name: httpd
        enabled: yes
        state: started
```



### 4. Ansible Lineinfile Module

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
- name: Install and Configure Apache Web Server on all App Servers
  hosts: all
  become: yes
  become_user: root
  
  tasks:
    - name: Install httpd package
      yum:
        name: httpd
        state: present
        
    - name: Start httpd service
      service:
        name: httpd
        state: started
    
    - name: Create the index.html file
      copy:
        dest: /var/www/html/index.html
        content: |
          This is a Nautilus sample file, created using Ansible!
              
    - name: Insert line at the beginning of the file
      lineinfile:
        path: /var/www/html/index.html
        line: "Welcome to xFusionCorp Industries!"
        insertbefore: BOF
        
    - name: Set the file permissions and owner to 'apache'
      file:
        path: /var/www/html/index.html
        owner: apache
        group: apache
        mode: "0744"
```



### 5. Ansible Replace Module

```bash
# thor@jump_host
cd ansible/
vi playbook.yml
ansible-playbook -i inventory playbook.yml
```
```yaml
# playbook.yml
---
- name: Replace strings on all app servers
  hosts: all
  become: yes
  
  tasks:
  - name: Replace text in blog.txt 
    replace:
      path: /opt/devops/blog.txt
      regexp: "xFusionCorp"
      replace: "Nautilus"
    when: inventory_hostname == "stapp01"

  - name: Replace text in story.txt 
    replace:
      path: /opt/devops/story.txt
      regexp: "Nautilus"
      replace: "KodeKloud"
    when: inventory_hostname == "stapp02"
      
  - name: Replace text in media.txt 
    replace:
      path: /opt/devops/media.txt
      regexp: "KodeKloud"
      replace: "xFusionCorp Industries"
    when: inventory_hostname == "stapp03"
```



### Test

1. Test

```bash
# thor@jump_host

```