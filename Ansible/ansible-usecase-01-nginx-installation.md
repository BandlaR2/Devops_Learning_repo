# Ansible Use Case 1: Install and Verify Nginx on 5 Servers

## Objective

Automate the installation and verification of Nginx on 5 servers using Ansible.

## Environment

* Control Node: macOS
* Automation Tool: Ansible
* Managed Hosts: 5 Servers (server1, server2, server3, server4, server5)

## Inventory File

```ini
[webservers]
server1 ansible_host=localhost ansible_connection=local
server2 ansible_host=localhost ansible_connection=local
server3 ansible_host=localhost ansible_connection=local
server4 ansible_host=localhost ansible_connection=local
server5 ansible_host=localhost ansible_connection=local
```

## Step 1: Verify Connectivity

```bash
ansible webservers -i inventory -m ping
```

Output:

```text
server1 | SUCCESS => pong
server2 | SUCCESS => pong
server3 | SUCCESS => pong
server4 | SUCCESS => pong
server5 | SUCCESS => pong
```

## Step 2: Install Nginx

Playbook: nginx.yml

```yaml
---
- hosts: webservers
  gather_facts: no

  tasks:
    - name: Install Nginx
      homebrew:
        name: nginx
        state: present
```

Run:

```bash
ansible-playbook -i inventory nginx.yml
```

## Step 3: Start Nginx Service

```bash
brew services start nginx
```

Verify:

```bash
brew services list
```

Output:

```text
nginx started
```

## Step 4: Verify Nginx Installation

Playbook: check_nginx.yml

```yaml
---
- hosts: webservers
  gather_facts: no

  tasks:
    - name: Check Nginx Version
      command: nginx -v
      register: nginx_version
      changed_when: false

    - name: Display Version
      debug:
        var: nginx_version.stderr
```

Run:

```bash
ansible-playbook -i inventory check_nginx.yml
```

Output:

```text
server1 -> nginx version: nginx/1.31.1
server2 -> nginx version: nginx/1.31.1
server3 -> nginx version: nginx/1.31.1
server4 -> nginx version: nginx/1.31.1
server5 -> nginx version: nginx/1.31.1
```

## Step 5: Verify Through Browser

Nginx was configured to listen on port 8080.

Open:

```text
http://localhost:8080
```

Expected Page:

```text
Welcome to nginx!
```

## Outcome

* Successfully automated Nginx installation using Ansible.
* Verified connectivity to all target hosts.
* Started the Nginx service.
* Verified the installed version using Ansible.
* Confirmed web server accessibility through the browser.

## Skills Demonstrated

* Inventory Management
* Ansible Playbooks
* Homebrew Module
* Command Module
* Debug Module
* Service Management
* Nginx Deployment
* Infrastructure Automation give file name