# Ansible Setup Guide

## Overview
This repository provides a comprehensive guide for setting up an Ansible Controller and configuring Ansible Nodes (Node 1 and Node 2). It includes step-by-step instructions for configuring SSH, creating users, and establishing seamless connectivity for effective automation using Ansible.

## Features
- Detailed steps for setting up the Ansible Controller.
- Instructions for configuring SSH and cloud-init settings.
- Guidelines for creating and managing the Ansible user on both the controller and nodes.
- Configuration of passwordless sudo and SSH key-based authentication.
- Testing connectivity and verifying Ansible setup with an inventory file.

## Follow the steps provided in `ansible_setup_guide.md` to set up your Ansible environment.

## Prerequisites
- A Linux-based system (e.g., RHEL 9.x or CentOS Stream 9).
- Administrative (root) access to the systems.
- SSH access between the controller and nodes.

Here’s a detailed explanation and corrected step-by-step guide for setting up an Ansible Controller and configuring Ansible Nodes (Node 1 and Node 2).

### Ansible Controller Setup
   ## 1. Install vim
       bash
       Copy code
       yum install vim -y
        ◦ vim is used to edit configuration files.
  ##  2. Edit SSH Server Configuration
        ◦ Open the sshd_config file:
          bash
          Copy code
          vim /etc/ssh/sshd_config
        ◦ Update the following line to allow root login:
          bash
          Copy code
          PermitRootLogin yes
        ◦ Save and exit the file.
  ##  3. Edit Cloud-Init SSH Configuration
        ◦ Open the cloud-init SSH configuration:
          bash
          Copy code
          vim /etc/ssh/sshd_config.d/50-cloud-init.conf
        ◦ Add or ensure the following line exists:
          bash
          Copy code
          PasswordAuthentication yes
        ◦ Save and exit the file.
 ##   4. Create Ansible User
        ◦ Add a new user:
          bash
          Copy code
          useradd ansible
        ◦ Set a password for the user:
          bash
          Copy code
          passwd ansible
##    5. Configure Sudo Access
        ◦ Open the sudoers file:
          bash
          Copy code
          vim /etc/sudoers
        ◦ Add the following line to give the ansible user passwordless sudo access:
          bash
          Copy code
          ansible ALL=(ALL) NOPASSWD: ALL
        ◦ Save and exit the file.
##    6. Restart SSH Service
       bash
       Copy code
       systemctl restart sshd
##    7. Generate SSH Keys
        ◦ Generate SSH key pairs for the ansible user:
          bash
          Copy code
          ssh-keygen
        ◦ Copy the SSH key to the nodes (replace <NODE_IP> with the actual node IP):
          bash
          Copy code
          ssh-copy-id ansible@<NODE_IP>
##    8. Install Required Packages
        ◦ Install necessary repositories:
          bash
          Copy code
          dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm
        ◦ Install ansible-core:
          bash
          Copy code
          yum install ansible-core -y

### Ansible Node 1 and Node 2 Setup
For each node, follow these steps:
 ##   1. Install vim
       bash
       Copy code
       yum install vim -y
##    2. Edit SSH Server Configuration
        ◦ Open the sshd_config file:
          bash
          Copy code
          vim /etc/ssh/sshd_config
        ◦ Ensure the following settings:
          bash
          Copy code
          PermitRootLogin yes
          PasswordAuthentication yes
        ◦ Save and exit the file.
##    3. Restart SSH Service
       bash
       Copy code
       systemctl restart sshd
##    4. Create Ansible User
        ◦ Add the ansible user:
          bash
          Copy code
          useradd ansible
        ◦ Set a password for the ansible user:
          bash
          Copy code
          passwd ansible
##    5. Configure Sudo Access
        ◦ Open the sudoers file:
          bash
          Copy code
          vim /etc/sudoers
        ◦ Add the following line:
          bash
          Copy code
          ansible ALL=(ALL) NOPASSWD: ALL
        ◦ Save and exit the file.
##    6. Allow SSH Access from the Controller
        ◦ Accept the SSH key from the controller (executed on the controller):
          bash
          Copy code
          ssh-copy-id ansible@<NODE_IP>

### Finalizing Ansible Setup
  ##  1. Edit the Inventory File on the Controller
        ◦ Add the nodes to the inventory file:
          bash
          Copy code
          vim /etc/ansible/hosts
        ◦ Example inventory file:
          ini
          Copy code
          [web]
          node1 ansible_host=<NODE1_IP>
          
          [db]
         node2 ansible_host=<NODE2_IP>
          
##   2. Test Ansible Connectivity
        ◦ Run a ping command to test Ansible:
          bash
          Copy code
          ansible all -m ping
        ◦ You should see a success message from both nodes.

Successful responses from all nodes indicate a proper configuration.
