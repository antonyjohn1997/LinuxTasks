### Automating Docker Installation on Ubuntu Using Ansible

## Introduction

In this guide, we will automate the installation and configuration of Docker on an Ubuntu system using Ansible. This approach ensures a repeatable, error-free setup and allows users to run Docker commands without needing sudo.

## Prerequisites

Before proceeding, ensure you have the following:

* Ubuntu OS (Tested on Ubuntu 20.04/22.04)

* Ansible installed (Instructions provided below)

* A user with sudo privileges

* SSH access (If setting up Docker on a remote machine)

## Step-by-Step Guide

* Step 1: Install Ansible

If Ansible is not already installed, install it using the following commands:

* sudo apt update
* sudo apt install ansible -y

## Verify the installation:

* ansible --version

## Step 2: Set Up the Ansible Project Directory

Create a directory for the playbook:

* mkdir -p ~/ansible-docker
* cd ~/ansible-docker

## Step 3: Create the Ansible Playbook

Create a new YAML file for the playbook:

* nano install_docker.yml

Copy and paste the following Ansible playbook:

---
- name: Install and configure Docker on Ubuntu
  hosts: localhost  # Runs locally
  become: true  # Executes with sudo privileges

  tasks:
    - name: Update package list
      apt:
        update_cache: yes

    - name: Install required dependencies
      apt:
        name:
          - apt-transport-https
          - ca-certificates
          - curl
          - software-properties-common
        state: present

    - name: Add Docker’s official GPG key
      shell: curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

    - name: Add Docker repositoransy
      shell: add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

    - name: Install Docker
      apt:
        name: docker-ce
        state: present
        update_cache: yes

    - name: Start and enable Docker service
      systemd:
        name: docker
        state: started
        enabled: yes

    - name: Create a new user and add to Docker group
      user:
        name: dockeruser  # Change to your username
        groups: docker
        append: yes

Save and exit (CTRL+X, then Y, then ENTER).

## Step 4: Run the Ansible Playbook

Execute the playbook with the following command:

ansible-playbook install_docker.yml --ask-become-pass

(You will be prompted for your sudo password.)

## Running the Ansible Playbook

    You run the Ansible playbook to install Docker and create a user (dockeruser).
    Ansible does not set a password for dockeruser, so it cannot log in with a password.

When Ansible runs, it asks for the sudo password of your currently logged-in Ubuntu user (antony in my case).
👉 You enter your Ubuntu user’s password to allow Ansible to install and configure Docker.


Step 5: Verify Docker Installation

After installation, verify that Docker is running:

* docker --version

Check Docker’s status:

* sudo systemctl status docker

## Switching to dockeruser

After running the playbook, check if the user dockeruser exists:

* cat /etc/passwd | grep dockeruser

Now, switch to dockeruser:

* sudo su - dockeruser

    The system asks for a password.
    Since dockeruser was created without a password, enter the password of your current Ubuntu  user (antony).
    Now, you are logged in as dockeruser.

## Run a test container:

* docker run hello-world

## Step 6: Run Docker Without sudo

If you added yourself to the Docker group, try running Docker without sudo:

* docker run hello-world

If you see a permission error, restart your session or run:

* newgrp docker

## Running Docker Commands as dockeruser

Since dockeruser was added to the Docker group (by Ansible), it can run Docker commands without sudo.
✅ Check if Docker is running:

* docker ps

👉 If it returns nothing, no containers are running.
✅ Check all containers (including stopped ones):

* docker ps -a

👉 This will show all containers, even if they are stopped.
✅ Check installed images:

* docker images

👉 This shows the Docker images available on your system.
✅ Pull an Ubuntu image from Docker Hub:

* docker pull ubuntu

👉 This downloads the Ubuntu image from Docker Hub.
✅ Run an Ubuntu container in interactive mode:

* docker run -it ubuntu bash

👉 This starts a new Ubuntu container and opens a bash shell inside the container.
✅ Check all containers again:

* docker ps -a

👉 This will now show the newly created Ubuntu container.

## Why dockeruser Can Run Docker Commands Without sudo

    Normally, Docker requires sudo (e.g., sudo docker ps).
    But Ansible added dockeruser to the Docker group, so it can run commands without sudo.
    To verify that dockeruser is in the Docker group:

* groups

👉 This should show docker.


## Conclusion

By automating the Docker installation with Ansible, we reduce manual effort and ensure consistency. This setup allows users to run Docker commands efficiently without requiring sudo access. 


