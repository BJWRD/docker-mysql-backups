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
To ensure that the MySQL DB password is not specified within the docker-compose.yml file we will be including the password within a Docker Secret instead.

As Docker Secrets is a Docker Swarm feature, our first step will be to ensure that the host is initialised as a Docker Swarm orchestrator. Specify the VM IP address within the command below -

    docker swarm init --advertise-addr 192.168.56.134

Enter Image 1

Then create a file including your MySQL DB password -

     vi my_secret_file

Once the file has been created the Docker Secret can be assigned to that file -

     docker secret create my_secret my_secret_file

Enter Image 2

List the Docker Secret for verification 

     docker secret ls

Enter Image 3

## Run MySQL Container
Before we begin the MySQL Setup, we need to ensure that Docker and Docker-Compose has been installed on the VM you are using. Please follow the steps within the 'Prerequisites' section to get started.

Once Docker and Docker-Compose has been installed, execute the following Docker-Compose command to start the MySQL container in detatched mode. This will host your MySQL instance.

      docker-compose up -d
      
## Trivy Image Scan
### 1. Install Trivy
Change the *trivy.sh* file permissions

      chmod 700 trivy.sh
      
Execute the Trivy Script (This will install the trivy software)

      ./trivy.sh
      trivy -v

Enter Image 1

### 2. Trivy Scan - MySQL Image 

      trivy image mysql:latest --severity MEDIUM,HIGH,CRITICAL > mysql_vulnerability_output
      
### 3. Review Vulnerabilites 
Now that Trivy has scanned the MySQL image, you are now in the position to be able to remediate any vulnerabilites which are associated with the image which have been picked up.

      cat mysql_vulnerability_output
      
Enter Image 2

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
