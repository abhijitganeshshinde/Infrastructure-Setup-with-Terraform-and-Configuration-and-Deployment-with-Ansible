# Infrastructure Setup with Terraform and Configuration and Deployment with Ansible

This project demonstrates the setup of infrastructure on AWS using Terraform and the configuration and deployment of applications using Ansible.

## Table of Contents

- [Part 1: Infrastructure Setup with Terraform](#part-1-infrastructure-setup-with-terraform)
- [Part 2: Infrastructure Setup with Ansible](#1-aws-setup-and-terraform-initialization)

  - [1. Web Server Setup](#2-web-server-setup)
  - [2. Database Server Setup](#3-database-server-setup)
  - [3. Application Deployment](#4-application-deployment)

- [Usage](#usage)
- [Notes](#notes)

## Part 1: Infrastructure Setup with Terraform

[How to Install Terraform on EC2 Ubuntu Instance](https://medium.com/@mudasirhaji/how-to-install-terraform-on-ec2-ubuntu-instance-407eb2279140): Follow this guide to install Terraform on an EC2 Ubuntu instance.

## Part 2: Infrastructure Setup with Ansible

[How to Install Ansible on EC2 Ubuntu Instance](https://techexpert.tips/ansible/ansible-installation-cloud-aws-ec2): Follow this guide to install Terraform on an EC2 Ubuntu instance.

### Web Server Setup

Write Ansible Playbook
Create a playbook (web_server_setup.yml) to install Node.js and clone the MERN application repository.

```yml
- name: Install Node.js and NPM
  hosts: web_servers
  become: yes
  tasks:
    - name: Install Node.js
      apt:
        name: nodejs
        state: present

    - name: Install NPM
      apt:
        name: npm
        state: present

- name: Clone MERN Application Repository
  hosts: web_servers
  become: yes
  tasks:
    - name: Clone Repository
      git:
        repo: <repository_url>
        dest: /path/to/clone
```

### Database Server Setup

Write Ansible Playbook
Create a playbook (db_server_setup.yml) to install and configure MongoDB.

```yml
- name: Install MongoDB
  hosts: db_servers
  become: yes
  tasks:
    - name: Add MongoDB Repository
      apt_repository:
        repo: deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu bionic/mongodb-org/4.4 multiverse
        state: present

    - name: Install MongoDB
      apt:
        name: mongodb-org
        state: present

    - name: Start MongoDB Service
      service:
        name: mongod
        state: started
        enabled: yes

    - name: Secure MongoDB
      template:
        src: mongod.conf.j2
        dest: /etc/mongod.conf
      notify: restart mongod
```

### Application Deployment

Write Ansible Playbook
Create a playbook (app_deployment.yml) to configure environment variables and start the Node.js application.

```yml
- name: Configure Environment Variables
  hosts: web_servers
  become: yes
  tasks:
    - name: Set Environment Variables
      lineinfile:
        path: /path/to/.env
        line: "KEY=VALUE"
        state: present

- name: Start Node.js Application
  hosts: web_servers
  become: yes
  tasks:
    - name: Start Application
      command: npm start
      args:
        chdir: /path/to/application
```

### Usage

- Clone this repository to your local machine.
- Navigate to the terraform directory and run terraform init to initialize the Terraform project.
- Run terraform apply to apply the Terraform configuration and provision the infrastructure on AWS.
- Navigate to the ansible directory and run the Ansible playbooks to configure and deploy applications on the provisioned infrastructure.

### Notes

- Ensure to replace placeholders with actual values and adapt configurations to your specific requirements and environment.
  Always follow AWS best practices for security and optimization.
  For detailed usage instructions and additional configurations, refer to the respective documentation of Terraform and Ansible.
