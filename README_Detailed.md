# Vagrant Ansible Task 

With these codes you will be able to install and configure fully automated KeyCloak and integrate with MySQL server.

## Getting Started

Below instructions will get you a copy of the project up and running on your local machine for development and testing purposes. 

### Prerequisites

This task was tested on Windows based host machine. However can be deployed on any platform that supports Vagrant and VirtualBox.

### Download and install 

Vagrant 2.1.5_x86_64.msi
VirtualBox 5.2.18-124319-Win.exe
Git-2.19.0-64-bit.exe

#### Installing

Open Git Bash console and create a new directory
$ mkdir vagrant_folder
$ cd vagrant_folder/
$ git clone https://github.com/salmanaghayev/ansible-task.git && cd ansible-task
Vagrant up command will install all required Operating Systems and execute playbooks.
$ vagrant up

## Running the manual test

If below link opens and requires username password and can be logged in with user: admin; pass: admin. It means everything is working
http://10.1.42.102:9990

## Detailed description of how process will happen

First file to be executed by vagrant up is vagrantfile. It deploys two Centos machines (one for keycloak application and the other for MySQL database) and one Ubuntu machine (for Ansible) on VirtualBox. Vagrantfile also will run scripts/install.sh on all machines, which sets hostnames, IPs, installs Java, does updates as well as creates vault_pass.txt which later will encrypt  varies data. 
In addition Vagrantfile will run scripts/callplaybooks.sh on Centos machine which in our case will be Ansible server. 
scripts/callplaybooks.sh has two lines. One line will execute DB playbook with required arguments and the other will run WEB playbook. 
tasks/deploy-db.yml playbook will install MySQL on first Centos machine, copy temps/my.cnf.j2 and overwrite /etc/my.cnf config file which states mysql bind address, set new mysql root password, create database, db user and pass for keyclock server and add mysql service to the startup.
tasks/deploy-web.yml playbook will download and install keycloak app  and mysql java connector. Playbook will also do reqired actions and eventually test is the app works.  At the end if we see below it means Keycloak app is working.
```
TASK [Fail if Management Interface is not in the page content] *****************\n
skipping: [webserver]
```
