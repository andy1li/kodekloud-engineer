# Ansible

## Level 1

### 1. Troubleshoot and Create Ansible Playbook

```bash
# thor@jump_host
vi /home/thor/ansible/inventory
> stapp02 ansible_host=172.16.238.11 ansible_user=steve ansible_ssh_pass=Am3ric@ ansible_ssh_common_args='-o StrictHostKeyChecking=no'

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
vi /home/thor/playbook/inventory
> stapp02 ansible_host=172.16.238.11 ansible_user=steve ansible_ssh_pass=Am3ric@ ansible_ssh_common_args='-o StrictHostKeyChecking=no'

ansible -m ping all -i /home/thor/playbook/inventory
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

> stapp01 ansible_host=172.16.238.10 ansible_user=tony ansible_ssh_pass=Ir0nM@n ansible_ssh_common_args='-o StrictHostKeyChecking=no'
> stapp02 ansible_host=172.16.238.11 ansible_user=steve ansible_ssh_pass=Am3ric@ ansible_ssh_common_args='-o StrictHostKeyChecking=no'
> stapp03 ansible_host=172.16.238.12 ansible_user=banner ansible_ssh_pass=BigGr33n ansible_ssh_common_args='-o StrictHostKeyChecking=no'

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



### 5. Create Files on App Servers using Ansible

```bash
# thor@jump_host
cd playbook/
vi inventory

vi inventory

> stapp01 ansible_host=172.16.238.10 ansible_user=tony ansible_ssh_pass=Ir0nM@n ansible_ssh_common_args='-o StrictHostKeyChecking=no'
> stapp02 ansible_host=172.16.238.11 ansible_user=steve ansible_ssh_pass=Am3ric@ ansible_ssh_common_args='-o StrictHostKeyChecking=no'
> stapp03 ansible_host=172.16.238.12 ansible_user=banner ansible_ssh_pass=BigGr33n ansible_ssh_common_args='-o StrictHostKeyChecking=no'

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

1. Test

```bash
# thor@jump_host

```
