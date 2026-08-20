# Ansible Automated Web Deployment 
 
> Automated Nginx web server deployment and configuration using Ansible, AWS EC2, and Ansible Roles. 
 
##  Project Overview 
 
This project demonstrates infrastructure automation using Ansible. 
 
A dedicated Ansible controller manages a remote Ubuntu EC2 server and automatically: 
 
- Installs and configures Nginx 
- Creates the application directory 
- Deploys a dynamic HTML webpage 
- Starts and manages the Nginx service 
- Restarts Nginx when configuration or application files change 
- Verifies the deployed website 
 
The project uses Ansible Playbooks, Roles, Jinja2 Templates, Handlers, and an Inventory to implement a reusable deployment workflow. 
 
## 🏗️ Architecture 
 
```text 
                    AWS EC2 
              ┌─────────────────┐ 
              │ Ansible         │ 
              │ Controller      │ 
              │                 │ 
              │ Playbooks       │ 
              │ Roles           │ 
              │ Inventory       │ 
              └────────┬────────┘ 
                       │ 
                       │ SSH 
                       ▼ 
              ┌─────────────────┐ 
              │ Ubuntu EC2      │ 
              │ Target Server   │ 
              │                 │ 
              │ ┌─────────────┐ │ 
              │ │    Nginx    │ │ 
              │ └──────┬──────┘ │ 
              │        │        │ 
              │        ▼        │ 
              │   Web Application│ 
              └─────────────────┘ 
## 🛠️ Technologies Used

- Ansible
- AWS EC2
- Ubuntu
- Nginx
- YAML
- Jinja2
- Git & GitHub
- SSH

## 📁 Project Structure

```text
ansible-lab/
├── inventory
├── inventory.ini
├── playbooks/
│   └── site.yml
├── roles/
│   └── webserver/
│       ├── handlers/
│       │   └── main.yml
│       ├── tasks/
│       │   └── main.yml
│       └── templates/
│           └── index.html.j2
├── first-playbook.yml
├── setup.yml
├── ansible.cfg
└── README.md
