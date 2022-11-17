# docker-mysql-backups
[MySQL](https://www.mysql.com/) Database, built by [Dockerfile](https://docs.docker.com/engine/reference/builder/), provisioned via [Docker-Compose](https://docs.docker.com/compose/install/) and hosted within a [Docker](https://www.docker.com/) container. Including a [Trivy](https://www.aquasec.com/products/trivy/) Docker Image scan, Docker Secrets setup and Database backup/restore.

## Architecture

Enter Image here

## Prerequisites
* Docker installation - [steps](https://docs.docker.com/engine/install/)
* Docker-Compose setup - [steps](https://docs.docker.com/compose/)
* Virtualbox installation - [steps](https://www.virtualbox.org/wiki/Downloads) 
* Trivy installation - [steps](https://aquasecurity.github.io/trivy/v0.34/getting-started/installation/)

## Docker/MySQL Container Installation
### 1. Clone the Git Repository
      sudo yum install git -y 
      cd /home
      git clone https://github.com/BJWRD/docker-mysql-backups
      cd docker-mysql-backups
      
## Docker Secrets
      
## Run MySQL Container
Before we begin the MySQL Setup, we need to ensure that Docker and Docker-Compose has been installed on the VM you are using. Please follow the steps within the 'Prerequisites' section to get started.

Once Docker and Docker-Compose has been installed, execute the following Docker-Compose command to start the MySQL container in detatched mode. This will host your MySQL instance.

      docker-compose up -d
      
## Trivy Image Scan

## MySQL Database Setup

## Connecting to MySQL WorkBench

## MySQL Database Backups



## Conclusion
In this project, we have.....

## List of tools/services used
* [Docker](https://www.docker.com/)
* [Dockerfile](https://docs.docker.com/engine/reference/builder/)
* [Docker-Compose](https://docs.docker.com/compose/)
* [Virtualbox](https://www.virtualbox.org) 
* [MySQL](https://www.mysql.com/)
* [Trivy](https://www.aquasec.com/products/trivy/)
