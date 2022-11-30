# docker-mysql-backups
[MySQL](https://www.mysql.com/) Database, provisioned and hosted via [Docker-Compose](https://docs.docker.com/compose/install/). Including a [Trivy](https://www.aquasec.com/products/trivy/) Image Scan, [Docker Secrets](https://docs.docker.com/engine/swarm/secrets/) implementation and then the MySQL Database Backups.

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
### 1. Docker Swarm Initialisation
To ensure that the MySQL DB password's are not specified within the docker-compose.yml file we will be including the DB password and the root DB password within Docker Secrets instead.

As Docker Secrets is a Docker Swarm feature, our first step will be to ensure that the host is initialised as a Docker Swarm orchestrator. Specify your VM IP address within the command below -

    docker swarm init --advertise-addr 192.168.56.134

<img width="819" alt="image" src="https://user-images.githubusercontent.com/83971386/204331563-95c2e5ab-a76a-4743-be6d-5945aa8d5929.png">

### 2. Creating Secrets
Once the VM has been Docker Swarm Initialised, you will need to create your external secrets (make sure to include the final dash `-`. It's easy to miss.) -

     echo "hello" | docker secret create db_password -
     echo "hello" | docker secret create db_root_password -

**NOTE:** Ensure you do not share the password above with anyone else.

<img width="635" alt="image" src="https://user-images.githubusercontent.com/83971386/204332519-55f077a1-ea3b-4f43-85d3-8f546b11e178.png">

### 3. Stack Deployment
Now deploy the stack using -

      docker stack deploy --compose-file=docker-compose.yml example
      
### 4. Verification
You can now verify that the Docker Container is running with the Secrets correctly assigned by entering the commands below -

      docker ps
      
<img width="903" alt="image" src="https://user-images.githubusercontent.com/83971386/204334083-8a189e9f-b3fc-415f-8faa-32fa780f6ed2.png">

      docker secret ls

<img width="606" alt="image" src="https://user-images.githubusercontent.com/83971386/204334405-2ebaea7a-bac7-4f6b-b520-64e898d10d69.png">

      docker service ls

<img width="582" alt="image" src="https://user-images.githubusercontent.com/83971386/204338270-25f74f79-a780-4222-a915-8c6b2e2f4bc3.png">
      
      docker exec -it <docker container ID> ls /run/secrets

<img width="580" alt="image" src="https://user-images.githubusercontent.com/83971386/204336989-e9bfefe0-9759-4dc1-9b3b-ba66cdde1a9c.png">

If all is well, the two secrets we created *db_password* and *db_root_password* should be within the containers */run/secrets* directory, following the stack deployment (see imagery above).
      
## Trivy Image Scan
Now that the mysql image has been pulled down onto the VM following the *docker stack deploy*, we will now be able to run a Trivy scan on the image to identify any vulnerabilities.

### 1. Install Trivy
Change the *trivy.sh* file permissions

      chmod 700 trivy.sh
      
Execute the Trivy Script (This will install the trivy software)

      ./trivy.sh
      trivy -v

<img width="315" alt="image" src="https://user-images.githubusercontent.com/83971386/204342988-2cf76a63-7307-4da2-b0f2-a280af07e5f2.png">

### 2. Trivy Scan - MySQL Image 

      trivy image mysql:latest --severity MEDIUM,HIGH,CRITICAL > mysql_vulnerability_output
      
<img width="611" alt="image" src="https://user-images.githubusercontent.com/83971386/204343266-0379f665-82e3-4bcd-824a-8b7632adfc3d.png">
      
### 3. Review Vulnerabilites 
Now that Trivy has scanned the MySQL image, you are now in the position to be able to remediate any vulnerabilites which are associated with the image which have been identified.

      cat mysql_vulnerability_output

## MySQL Database Access
Now that the MySQL Docker Container is running, the first step is to login using your root account and password -

      mysql -u root -p
      
From here onwards it's up to you how you would like to tailor the Database to your own needs, but to help you get started, I've listed some key MySQL commands below which may be of use -

      HELP;
      
      SHOW databases;
      
      CREATE database <db name>;
      
      use <db name>;
      
      drop <db name>;
      
      SHOW tables;
      
      CREATE table <table name>;
      
      use <table name>;
      
      drop <table name>;
      
For more MySQL commands, please visit the following site - https://w3cschoool.com/tutorial/mysql-commands-cheat-sheet

## Connecting to MySQL WorkBench
To access the Database via MySQL WorkBench follow the steps below -

### 1. Download MySQL WorkBench
You can download MySQL WorkBench from the following Oracle site - https://dev.mysql.com/downloads/workbench/

### 2. Setup and Connection
Once downloaded and installed on your machine, specify the following connection details to establish a connection to the DB 

<img width="444" alt="image" src="https://user-images.githubusercontent.com/83971386/204785257-07298d11-a68c-47eb-88dc-cfda331095de.png">

**NOTE:** You will need to alter the IP address to your own VM's IP.

<img width="204" alt="image" src="https://user-images.githubusercontent.com/83971386/204785339-83626d93-6abf-4963-869b-3e40f3a9f9d4.png">

### 3. Operating the GUI
Access the MySQL WorkBench GUI and you can begin to execute any query that you may or may not need -

<img width="506" alt="image" src="https://user-images.githubusercontent.com/83971386/204785632-1ef62fee-5d7a-4844-8323-87b749602193.png">


## MySQL Database Backups
To perform a MySQL Database dump, execute the following command -

      docker exec -it 7b4efde3d7c7 /usr/bin/source -u root -p <password> mysql > /tmp/backup_db.sql

This will result in a backup file which can be found within your host's */tmp* directory -

<img width="806" alt="image" src="https://user-images.githubusercontent.com/83971386/204821076-6ebc22fe-eb12-4fe7-bf4c-4c59203d8552.png">

Ideally, you would want to create a Cronjob including this command so your MySQL Database is backed up on a daily.

## Conclusion
In this project, we have provisioned a local MySQL Docker Container with it's passwords being masked by Docker Secrets. The vulnerabilites associated with the MySQL image would have been identified using Trivy, MySQL WorkBench has been used to connect to the Database and Database Backups have also been performed.

## List of tools/services used
* [Docker](https://www.docker.com/)
* [Docker-Compose](https://docs.docker.com/compose/)
* [Docker Swarm](https://docs.docker.com/engine/swarm/)
* [Virtualbox](https://www.virtualbox.org) 
* [MySQL](https://www.mysql.com/)
* [Trivy](https://www.aquasec.com/products/trivy/)

## Useful Links -
* https://stackoverflow.com/questions/42139605/how-do-you-manage-secret-values-with-docker-compose-v3-1
* https://w3cschoool.com/tutorial/mysql-commands-cheat-sheet
* https://dev.mysql.com/downloads/workbench/
* [https://stackoverflow.com/questions/33827342/how-to-connect-mysql-workbench-to-running-mysql-inside-docker](https://stackoverflow.com/questions/33827342/how-to-connect-mysql-workbench-to-running-mysql-inside-docker#:~:text=1%20Specify%20mysql%20configuration%20block%20in%20your%20docker-compose.yml.,your%20MySQL%20Workbench%20provide%20the%20connection%20details.%20)
