# Ansible Jenkins Role README
### Overview
This Ansible role installs a specific version of Jenkins on **CentOS (RedHat family)** or **Ubuntu (Debian family)**, or both simultaneously via inventory groups. It supports version-specific installation from official repositories, variable-driven configuration, Jinja-templated config files (/etc/sysconfig/jenkins or /etc/default/jenkins), and dedicated handlers for service restarts.

### Key Features:
```
OS independent (CentOS/RHEL & Ubuntu/Debian)

Version-specific Jenkins (e.g., 2.426.3)

All config variablelized (port, Java opts, home dir)

Jinja2 templates for dynamic config files

Handlers in separate file (not inline with tasks)

Flexible execution on single OS or both
```
```
Role Structure
text
jenkins-role/
├── defaults/
│   └── main.yml          # Default variables
├── handlers/
│   └── main.yml          # Service restart handler
├── meta/
│   └── main.yml          # Role metadata (add platforms: [CentOS, Ubuntu])
├── tasks/
│   └── main.yml          # Main tasks (install Java, repo, Jenkins, template)
├── templates/
│   └── jenkins.j2        # Config template with {{ vars }}
├── vars/
│   └── main.yml          # Fixed vars (optional)
└── README.md             # This file
```
<img width="1701" height="839" alt="Pasted image (4)" src="https://github.com/user-attachments/assets/075577b5-cd47-477f-bba2-7b2b408f63d7" />

### Requirements
```
Ansible 2.9+

Target hosts: CentOS 7/8/9 or Ubuntu 20.04/22.04

Root/sudo access

Internet for repo access
```
### Variables
Override in playbook or group_vars. Defaults in defaults/main.yml:

<img width="1257" height="210" alt="Pasted image (15)" src="https://github.com/user-attachments/assets/b1719ba6-6be2-4bff-acde-69d71ad18f29" />

### Dependencies
```
None. Role handles Java installation.
```
### Installation
Clone or copy role to roles/jenkins-role/

Or install from Galaxy: ansible-galaxy install geerlingguy.jenkins (adapt as needed)

#### Example Inventory
```
inventory:
```
<img width="1257" height="268" alt="Pasted image (16)" src="https://github.com/user-attachments/assets/4c458ba8-3874-4e69-b01e-2e9bb183fdb4" />

#### Example Playbook
site.yml (run on all, CentOS only, or Ubuntu only):

```
---
- name: Install Jenkins on all servers
  hosts: all
  become: yes
  roles:
    - jenkins
```
<img width="1257" height="178" alt="Pasted image (17)" src="https://github.com/user-attachments/assets/2d9b4501-273f-4e9c-b0a8-0676a2fe6dde" />


#### Usage
```
ansible-playbook -i inventory site.yml 
```
#### Expected Output:

<img width="1701" height="839" alt="Pasted image" src="https://github.com/user-attachments/assets/524ec4e0-328f-49ad-a186-7c9ff5eec23c" />
<img width="1701" height="839" alt="Pasted image (2)" src="https://github.com/user-attachments/assets/02ca7540-7ac8-4e88-91e4-97592cc9621d" />
<img width="1658" height="457" alt="Pasted image (3)" src="https://github.com/user-attachments/assets/86e9c810-4f8b-4991-9d4a-d2b17c13fd18" />

#### Templates
templates/jenkins.j2 example:


<img width="1334" height="278" alt="Pasted image (18)" src="https://github.com/user-attachments/assets/7bdbfb8d-50fd-4f21-8841-808ac18750a7" />

#### Handlers
```
handlers/main.yml:
```
text
- name: restart jenkins
  service:
    name: jenkins
    state: restarted

<img width="1334" height="197" alt="Pasted image (19)" src="https://github.com/user-attachments/assets/27b5b548-001f-48c4-89e4-c3e9810dd0f7" />

## Verification
### Post-install:

text
curl http://localhost:{{ jenkins_port }}
systemctl status jenkins
Jenkins unlocks via initial admin password in /var/lib/jenkins/secrets/initialAdminPassword.

<img width="1701" height="839" alt="Pasted image (14)" src="https://github.com/user-attachments/assets/732bc177-bbf9-47f8-a5a8-cc0cb2a82d79" />

