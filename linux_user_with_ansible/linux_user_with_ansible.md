##Create Linux user with Ansible
## localhost

Setting Up Ansible on Your Ubuntu Machine

1️⃣ Install Ansible
Run the following command:

* sudo apt update && sudo apt install -y ansible

2️⃣ Check if Ansible is Installed

* ansible --version

Running the Task on a Single Machine (Localhost)

Instead of using a separate server, modify the inventory file to use localhost.
1️⃣ Create Inventory File (inventory.ini)
* nano inventory.ini

[localhost]
127.0.0.1 ansible_connection=local


paste the code 

This tells Ansible to manage only the local machine.

2️⃣ Create Ansible Playbook (create-user.yml)

* nano create-user.yml 

- name: Create Linux Users
  hosts: localhost
  become: yes
  vars_files:
    - user-list-var.yml
  tasks:
    - name: Create users
      user:
        name: "{{ item.name }}"
        uid: "{{ item.uid }}"
        group: "{{ item.group }}"
        password: "{{ item.password }}"
        update_password: "on_create"
        comment: "{{ item.comment }}"
      with_items: "{{ users }}"



paste the code 

3️⃣ Create User List File (user-list-var.yml)

* nano user-list-var.yml

users:
  - { name: 'user01', uid: 1001, group: 'users', password: "{{ 'passw0rd' | password_hash('sha512') }}", update_password: 'on_create', comment: 'Test User 01' }

4️⃣ Run the Playbook

* ansible-playbook -i inventory.ini create-user.yml --ask-become-pass

    --ask-become-pass prompts for sudo password.
    
## Verify the Users Were Created

Run:

cat /etc/passwd | grep user01
    
    
    
* su - user01
  type the password
  
  
   
