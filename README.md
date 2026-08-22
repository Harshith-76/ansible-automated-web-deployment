# Ansible Automated Web Deployment

> Automated Nginx web server deployment and configuration using Ansible, AWS EC2, Ansible Roles, Jinja2 Templates, and Handlers.

## Project Overview

This project demonstrates **Infrastructure as Code (IaC)** by using Ansible to automate the deployment and configuration of a web server on an AWS EC2 instance.

A dedicated **Ansible Controller** connects to a remote Ubuntu EC2 instance through SSH and automatically:

- Installs Nginx
- Updates the package index
- Creates the application directory
- Deploys a dynamic HTML webpage using a Jinja2 template
- Starts and manages the Nginx service
- Restarts Nginx when required
- Verifies the deployed application

The project is structured using **Ansible Roles** to make the deployment reusable and maintainable.

---

## Architecture

```text
                    AWS EC2
              ┌───────────────────┐
              │ Ansible Controller│
              │                   │
              │  Inventory        │
              │  Playbooks        │
              │  Roles            │
              │  Jinja2 Templates │
              │  Handlers         │
              └─────────┬─────────┘
                        │
                        │ SSH
                        ▼
              ┌───────────────────┐
              │   Ubuntu EC2      │
              │   Target Server   │
              │                   │
              │  ┌─────────────┐  │
              │  │    Nginx    │  │
              │  └──────┬──────┘  │
              │         │         │
              │         ▼         │
              │   Web Application │
              └───────────────────┘
```

### Deployment Flow

```text
Developer
    │
    ▼
GitHub Repository
    │
    ▼
Ansible Controller
    │
    │ SSH
    ▼
Ubuntu EC2 Target
    │
    ├── Install Nginx
    ├── Create Application Directory
    ├── Deploy Jinja2 Template
    ├── Start Nginx
    └── Restart Nginx when required
            │
            ▼
       Deployed Website
```

---

## Technologies Used

- **Ansible** - Infrastructure automation and configuration management
- **AWS EC2** - Cloud virtual machines
- **Ubuntu** - Operating system for the controller and target server
- **Nginx** - Web server
- **YAML** - Ansible playbooks and configuration
- **Jinja2** - Dynamic HTML templating
- **SSH** - Secure communication between controller and target
- **Git** - Version control
- **GitHub** - Source code hosting

---

## Project Structure

```text
ansible-lab/
│
├── inventory
├── inventory.ini
├── ansible.cfg
│
├── playbooks/
│   └── site.yml
│
├── roles/
│   └── webserver/
│       ├── handlers/
│       │   └── main.yml
│       │
│       ├── tasks/
│       │   └── main.yml
│       │
│       ├── files/
│       │
│       └── templates/
│           └── index.html.j2
│
├── first-playbook.yml
├── setup.yml
│
└── README.md
```

---

## Ansible Role

The `webserver` role separates the deployment logic into reusable components.

### Tasks

The role's task file is responsible for:

1. Gathering system information
2. Updating the APT package index
3. Installing required packages
4. Creating the application directory
5. Deploying the website template
6. Ensuring Nginx is running

### Templates

The Jinja2 template dynamically generates the deployed webpage.

The webpage displays information such as:

- Server hostname
- Operating system
- Ansible-managed deployment information

### Handlers

Handlers are triggered when a task changes something that requires a service restart.

For example:

```text
Template changes
       │
       ▼
Handler triggered
       │
       ▼
Restart Nginx
```

This prevents unnecessary service restarts when nothing has changed.

---

## Inventory

The target EC2 instance is defined in `inventory.ini`.

Example:

```ini
[target]
<EC2_PRIVATE_IP> ansible_user=ubuntu
```

The private IP should be replaced with the private IP address of the target EC2 instance.

---

## How It Works

### 1. Test Ansible Connectivity

Before deployment, connectivity to the target server can be tested with:

```bash
ansible all -i inventory.ini -m ping
```

Expected result:

```text
SUCCESS => {
    "ping": "pong"
}
```

---

### 2. Run the Deployment

The main deployment playbook is executed using:

```bash
ansible-playbook -i inventory.ini playbooks/site.yml
```

Ansible then connects to the target EC2 instance and executes the `webserver` role.

---

### 3. Nginx Installation

Ansible automatically installs Nginx on the target Ubuntu server.

The deployment does not require manually logging into the target server and installing the web server.

---

### 4. Deploy the Website

The Jinja2 template:

```text
roles/webserver/templates/index.html.j2
```

is rendered and deployed to the target server.

The resulting webpage contains dynamically generated server information.

---

### 5. Manage Nginx

Ansible ensures that Nginx is running:

```bash
ansible all -i inventory.ini -m shell -a "systemctl is-active nginx"
```

Expected result:

```text
active
```

---

### 6. Verify the Website

The deployed website can be tested directly from the Ansible controller:

```bash
ansible all -i inventory.ini -m shell -a "curl -s http://localhost"
```

This returns the HTML generated by the deployed application.

The website can also be accessed through the public IP address of the target EC2 instance.

---

## Example Deployment

A successful deployment produces an Ansible result similar to:

```text
PLAY RECAP
172.31.25.62 : ok=7 changed=2 unreachable=0 failed=0
```

The deployed webpage confirms that the application was configured automatically by Ansible.

Example:

```text
Deployed using Ansible

This website was automatically configured by Ansible.

Server: ip-172-31-25-62
Operating System: Ubuntu
Managed by: Ansible Controller
```

---

## Idempotency

One of the key advantages of Ansible is **idempotency**.

Running the deployment multiple times does not repeatedly change resources that are already in the desired state.

For example:

```bash
ansible-playbook -i inventory.ini playbooks/site.yml
```

After the initial deployment, running the playbook again should result in most tasks reporting:

```text
ok
```

instead of:

```text
changed
```

This demonstrates that Ansible is maintaining the desired configuration rather than blindly executing commands.

---

## Verification Commands

### Test connectivity

```bash
ansible all -i inventory.ini -m ping
```

### Check Nginx status

```bash
ansible all -i inventory.ini -m shell -a "systemctl is-active nginx"
```

### Check deployed webpage

```bash
ansible all -i inventory.ini -m shell -a "curl -s http://localhost"
```

### Check Nginx version

```bash
ansible all -i inventory.ini -m shell -a "nginx -v"
```

---

## Key Ansible Concepts Demonstrated

This project demonstrates practical usage of:

- Ansible Inventory
- Ansible Playbooks
- Ansible Roles
- Tasks
- Handlers
- Jinja2 Templates
- Variables
- SSH-based remote execution
- Package management
- Service management
- Infrastructure as Code
- Idempotent configuration management

---

## What I Learned

Through this project, I gained hands-on experience with:

- Setting up an Ansible controller
- Managing remote Linux servers using Ansible
- Configuring AWS EC2 instances
- Creating reusable Ansible Roles
- Using Jinja2 templates for dynamic configuration
- Using handlers to manage service restarts
- Automating Nginx installation and configuration
- Structuring an Infrastructure-as-Code project
- Verifying deployments through Ansible
- Managing the project using Git and GitHub

---

## Future Improvements

Possible improvements to this project include:

- Add multiple EC2 target servers
- Introduce Ansible Vault for secrets management
- Add environment-specific inventories
- Add Nginx configuration templates
- Add automated testing
- Integrate the deployment with a CI/CD pipeline
- Add Docker-based application deployment
- Add monitoring and logging
- Deploy the infrastructure using Terraform

---

## Author

**Harshith-76**


