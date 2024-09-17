# Ansible Short Notes for DevOps Engineers

## Introduction to Ansible

Ansible is an open-source automation tool used for configuration management, application deployment, and orchestration.

- It follows a declarative approach, where you define the desired state of the system rather than scripting specific commands.
- Ansible uses SSH for communication with target systems, making it agentless and easy to set up.

### Key Concepts:

1. **Inventory**: The inventory file lists the target hosts on which Ansible will run tasks. It can be a static file or generated dynamically.
2. **Playbooks**: Playbooks are YAML files that define a set of tasks and configurations to be executed on target hosts.
3. **Tasks**: Tasks are the individual units of work in Ansible. They represent actions to be performed on target hosts.
4. **Modules**: Ansible provides a wide range of modules for various tasks, such as package installation, file manipulation, service management, etc.
5. **Roles**: Roles are reusable units of playbooks. They encapsulate related tasks, handlers, variables, and files into a directory structure.

## Installation of Ansible

```bash
sudo apt-add-repository ppa:ansible/ansible
sudo apt update
sudo apt install ansible
```

Edit the Ansible hosts file:
```bash
sudo nano /etc/ansible/hosts
```

Add servers to the hosts file:
```ini
[servers]
server1 ansible_host=<Public IP>
server2 ansible_host=<Public IP>
server3 ansible_host=<Public IP>

[all:vars]
ansible_python_interpreter=/usr/bin/python3
ansible_user=ubuntu
ansible_ssh_private_key_file=/<file>
```

List the inventory:
```bash
ansible-inventory --list -y
```

## Ansible Ad-hoc Commands vs Modules

- Ad-hoc commands are great for tasks you repeat rarely.
- Ansible modules are units of code that can control system resources or execute system commands.

Examples:
- Using modules (-m):
  ```bash
  ansible all -m ping -u ubuntu
  ```

- Using ad-hoc commands (-a):
  ```bash
  ansible all -a "df -h" -u ubuntu
  ansible servers -a "uptime" -u ubuntu
  ```

## Ansible Playbooks

### Simple Date Playbook

```yaml
---
- name: Date playbook
  hosts: servers
  tasks:
    - name: this will show the date
      command: date
```

### Playbook to Install Nginx

```yaml
---
- name: This playbook will install nginx
  hosts: servers
  become: yes
  tasks:
    - name: install nginx
      apt:
        name: nginx
        state: latest
    - name: start nginx
      service:
        name: nginx
        state: started
        enabled: yes
```

### Playbook to Install Tools with Conditional Statements

```yaml
---
- name: this will install based on os
  hosts: servers
  become: yes
  tasks:
    - name: install Docker
      apt:
        name: docker.io
        state: latest
    - name: install aws cli
      apt:
        name: awscli
        state: latest
      when: ansible_distribution == 'Debian' or ansible_distribution == 'Ubuntu'
```

Thank You!
