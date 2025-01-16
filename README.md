# Ansible Setup Guide

## Overview
This repository provides a comprehensive guide for setting up an Ansible Controller and configuring Ansible Nodes (Node 1 and Node 2). It includes step-by-step instructions for configuring SSH, creating users, and establishing seamless connectivity for effective automation using Ansible.

## Features
- Detailed steps for setting up the Ansible Controller.
- Instructions for configuring SSH and cloud-init settings.
- Guidelines for creating and managing the Ansible user on both the controller and nodes.
- Configuration of passwordless sudo and SSH key-based authentication.
- Testing connectivity and verifying Ansible setup with an inventory file.

## Prerequisites
- A Linux-based system (e.g., RHEL 9.x or CentOS Stream 9).
- Administrative (root) access to the systems.
- SSH access between the controller and nodes.

## Here’s a detailed explanation and corrected step-by-step guide for setting up an Ansible Controller and configuring Ansible Nodes (Node 1 and Node 2).
## A. Launch EC2 instances
![Screenshot from 2025-01-16 23-02-16](https://github.com/user-attachments/assets/d9458094-9eb2-45cc-a0f3-41338550f8c6)

## B. Ansible Controller Setup
   ## 1. Install vim
 
 ![Screenshot from 2025-01-16 23-06-15](https://github.com/user-attachments/assets/72481bce-2cf2-4b5f-bac1-1f1aa030d8c8)
       
   ◦ vim is used to edit configuration files.

  ##  2. Edit SSH Server Configuration
   ◦ Open the sshd_config file:
          
![Screenshot from 2025-01-16 23-09-13](https://github.com/user-attachments/assets/a94c5ffe-d915-41fd-a992-19a01e1867ea)

 ◦ Update the following line to allow root login:
 
          PermitRootLogin yes
   ◦ Save and exit the file.
 
  ##  3. Edit Cloud-Init SSH Configuration
 ◦ Open the cloud-init SSH configuration:
          
   ![Screenshot from 2025-01-16 23-10-42](https://github.com/user-attachments/assets/6636e657-6f66-4d7c-a670-4685a78ff172)

 ◦ Add or ensure the following line exists:
 
          PasswordAuthentication yes
 ◦ Save and exit the file.

 ##   4. Create Ansible User
   ◦ Add a new user & Set a password :
   
![Screenshot from 2025-01-16 23-14-16](https://github.com/user-attachments/assets/a880490f-903c-4ee2-9be3-e51aa2cdd5c6)


##    5. Configure Sudo Access
   ◦ Open the sudoers file:
         
![Screenshot from 2025-01-16 23-19-26](https://github.com/user-attachments/assets/103529b8-c4b1-4b34-b0d3-e3a658ba9890)

  ◦ Add the following line to give the ansible user passwordless sudo access:
         
       ansible ALL=(ALL) NOPASSWD:ALL
 ![Screenshot from 2025-01-16 23-18-45](https://github.com/user-attachments/assets/a2663f33-1f22-459e-9a4b-bfd22803c0d6)

 ◦ Save and exit the file.

##    6. Restart SSH Service

![Screenshot from 2025-01-16 23-20-19](https://github.com/user-attachments/assets/0fed308a-8b91-4e96-b40c-859947a571ab)



##    7. Install Required Packages
       
 ◦ Register the subscribe-manager of redhat :

   ![Screenshot from 2025-01-16 23-31-50](https://github.com/user-attachments/assets/0a3225a9-1fce-446b-b34d-a6598b0df58a)

        
 ◦ Install necessary repositories:
      
![Screenshot from 2025-01-16 23-34-05](https://github.com/user-attachments/assets/a51a253e-c8b5-43f6-aba7-bf5c821b9799)

![Screenshot from 2025-01-16 23-35-31](https://github.com/user-attachments/assets/7c40de2a-b2a8-480e-9202-815e5399a8c1)

![Screenshot from 2025-01-16 23-36-33](https://github.com/user-attachments/assets/1b56e3ba-3370-49a2-b127-4408a801ba2e)
         
◦ Install ansible-core:
   
![Screenshot from 2025-01-16 23-38-51](https://github.com/user-attachments/assets/f8c4e173-8d55-45b7-96cd-9213a73bb924)

      

##    8. Generate SSH Keys
  ◦ Generate SSH key pairs for the ansible user:

![Screenshot from 2025-01-16 23-41-58](https://github.com/user-attachments/assets/3bc2a4f2-f8ad-48d7-be43-ae0599a8ff85)

          
   ◦ Copy the SSH key to the nodes after compelete Ansible Node 1 and Node 2 Setup(replace <NODE_IP> with 
     the actual node IP):
         
          ssh-copy-id ansible@<NODE_IP>
![Screenshot from 2025-01-17 00-15-16](https://github.com/user-attachments/assets/d2a277c5-a73a-40a4-942d-fc65c6fda71e)
          
## C. Ansible Node 1 and Node 2 Setup
For each node, follow these steps:
 ##   1. Install vim

![Screenshot from 2025-01-16 23-48-59](https://github.com/user-attachments/assets/d18a1143-00f4-429d-9708-5693b8911359)


##    2. Edit SSH Server Configuration
  ◦ Open the sshd_config file:

![Screenshot from 2025-01-16 23-51-10](https://github.com/user-attachments/assets/3f49dfb7-a552-4278-b35d-e32e6fb74d95)
      
  ◦ Ensure the following settings:
         
          PermitRootLogin yes
          PasswordAuthentication yes
        
   ◦ Save and exit the file.

##   3. Edit Cloud-Init SSH Configuration
 ◦ Open the cloud-init SSH configuration:
          
![Screenshot from 2025-01-16 23-57-31](https://github.com/user-attachments/assets/b295c12d-3374-4994-80cd-0d7c74d6cefa)

 ◦ Add or ensure the following line exists:
                                          
     PasswordAuthentication yes
 ◦ Save and exit the file.  

##    4. Restart SSH Service

![Screenshot from 2025-01-16 23-58-15](https://github.com/user-attachments/assets/f0e0f744-fac9-46ef-8db9-3912235a75a3)


##    5. Create Ansible User
   ◦ Add the ansible user & Set a password:
         
![Screenshot from 2025-01-17 00-17-51](https://github.com/user-attachments/assets/02c42842-8100-4ed9-ba0b-f7219b16e84d)



##    6. Configure Sudo Access

◦ Open the sudoers file:

![Screenshot from 2025-01-17 00-02-56](https://github.com/user-attachments/assets/ebc98233-499f-4414-b401-5d3c41e54f60)
         

◦ Add the following line:

![Screenshot from 2025-01-17 00-01-55](https://github.com/user-attachments/assets/3c622c59-ef32-48fb-a105-9b0ea118d92f)

◦ Save and exit the file.

##    7. Allow SSH Access from the Controller

◦ Accept the SSH key from the controller (executed on the controller):

   ![Screenshot from 2025-01-17 00-19-20](https://github.com/user-attachments/assets/aa2ef1ca-cd29-433b-b329-427d31c8da88)
      

## D. Finalizing Ansible Setup
##  1. Edit the Inventory File on the Controller
◦ Add the nodes to the inventory file:

![Screenshot from 2025-01-17 00-25-08](https://github.com/user-attachments/assets/27204ddb-46de-497a-8653-b7801ff4b11c)

◦ Example inventory file:
      
![Screenshot from 2025-01-17 00-24-11](https://github.com/user-attachments/assets/8e8ad571-4de9-4e04-8962-4993d079ee2d)

![Screenshot from 2025-01-17 00-27-26](https://github.com/user-attachments/assets/da41cce9-1b13-4892-acef-c77724902a20)


##   2. Test Ansible Connectivity
◦ Run a  command to test Ansible:
        
![Screenshot from 2025-01-17 00-41-59](https://github.com/user-attachments/assets/a8bcc7ef-7fa8-4207-949e-1ee186c1e188)

      
◦ You should see a success create a directory from both nodes.

![Screenshot from 2025-01-17 00-44-35](https://github.com/user-attachments/assets/2acfd984-d4e4-4863-83b7-e45121a6d66f)

![Screenshot from 2025-01-17 00-43-42](https://github.com/user-attachments/assets/36c564e3-6dcb-46d8-bcec-c5df8d34a3b7)


Successful responses from all nodes indicate a proper configuration.
