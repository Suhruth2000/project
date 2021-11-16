# Aganitha Mini-Project Report
Aremanda Sri Sai Suhruth
IMT2018012
IIIT-B
# Introduction
The aim of this mini project is to deploy Jupyter and Postgresql docker containers in the cloud to explore and draw inferences of any dataset.
# Installation and Configuration
1. Cloud Instance: Create a t2.micro instance on AWS.
2. Deploy Jupyter Container.
3. Deploy Postgresql Container.
4. Link Jupyter and Postgresql Containers.
# Cloud Instance
1. Create an AWS account.
2. Click on Instances in the AWS dashboard and then launch instance.
3. Select Ubuntu Server 20.04 LTS image.
4. Select t2.micro instance and then click on review and launch.
5. Click on Edit security groups in the review page.
6. Create the following Rule
   1. Type- SSH
   2. Protocol- TCP
   3. Port Range- 22
   4. Source- My IP
7. Launch the instance.
8. Create a new SSH key pair in the pop up and download the private key file.
9. Connect to the instance using SSH through the terminal.
   1. Ssh -i “private-key” username@ip
10. Install Docker
   1. Sudo apt-get update
   2. sudo apt-get install \ ca-certificates \ curl \ gnupg \ lsb-release
   3. curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
   4. echo \  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
   5. sudo apt-get update
   6. sudo apt-get install docker-ce docker-ce-cli containerd.io
11. Install ngrok to access the GUI of the Jupyter notebook instance. 
   1. Create an account on ngrok
   2. wget https://bin.equinox.io/c/4VmDzA7iaHb/ngrok-stable-linux-amd64.zip
   3. sudo apt install unzip
   4. unzip ngrok-stable-linux-amd64.zip
   5. Connect your account with the authtoken given in the ngrok dashboard.


# Jupyter Notebook Docker Container
1. docker volume create jupyter (For persistent memory)
2. docker run -v jupyter:/app -p 8888:8888 jupyter/scipy-notebook:33add21fab64
3. Copy the token for login.
4. Open a new terminal and ssh to the instance like before.
5. ./ngrok http -region ap 8888 > /dev/null &
6. curl http://127.0.0.1:4040/api/tunnels
7. Copy the https url generated to access the jupyter notebook GUI.
# Postgresql Docker Container
1. docker pull postgres
2. sudo docker run -d -p 5432:5432 --name postgres -e POSTGRES_PASSWORD=<password> postgres
3. wget https://github.com/Suhruth2000/project/blob/main/diabetes.csv
4. sudo docker cp ./diabetes.csv postgres:./
5. sudo docker exec -it postgres bash
6. psql -h localhost -U postgres
7. CREATE DATABASE <db_name>;
8. \c <db_name>
9. CREATE TABLE <table_name> (Column-A, Column-B …)
10. \copy <table_name>(Column-A, Column-B …) from '/path/to/database.csv' CSV HEADER;
11. exit
# Link Jupyter and Postgresql
1. docker inspect postgres | grep IPAddress
2. Copy the IP address so as to connect postgresql to jupyter notebook. 
# Exploration
1. Open the ngrok link to Jupyter notebook.
2. Enter the token saved before to login
3. Create a new python notebook.
4. Write SQL queries and execute the notebook.
