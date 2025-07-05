
### Project Overview

This is a project that builds a python docker image using dockerfile and docker installed on the jenkins server. This docker image was pushed into elastic container registry (ECR), using jenkins pipeline,while pushing the containerised docker image  in ECR to aws cloud managed kubernetes cluster(aws-eks)
Initially, i had to provision the jenkins server using terraform (IAC).

### Tools Stack

i. AWS CLI - installed on the jenkins server,enable you to interface with AWS infrastructure.

ii. AWS configure - to insert Access-key and Secret-key, including Region and json format.

iii. Docker - installed on jenkins server, to enable you build docker images

iv. Terraform - installed locally on your machine, to enable you provision an ec2 instance called jenkins-server

v. ECR - created within AWS infrastructure to house the docker image

vi. Jenkins - installed and configured to run jenkins pipeline.

vii. Vscode - code editor installed locally on your machine.

viii. GitHub - a repository for your project code

https://github.com/clement2019/deploy-python-ecr-aws-eks.git

### Project Workflow
============
#### STAGE 1
==============
#### step 1
Created a python application,
collected all the project dependencies or libraries for the project and stored them inside "requirements.txt"

pip freeze >> requirements.txt

#### Step 2

run and test the project locally , to ensure it is working
i. $ python main.py

ii. localhost:5002

created a dockerfile to build docker image for the project

<img width="928" height="442" alt="Image" src="https://github.com/user-attachments/assets/7d2e4b58-7d21-455c-a9d2-3a8b1a07dd52" />


to build the application container locally

$ docker run --name seb-container -dp port 5002:5002 <imagename>:latest

to exec into the container

 $ docker exec seb-container -- sh

 $ exit

#### Step 3

run the following command to build the docker image in the jenkins pipeline.

dockerImage = docker.build "${IMAGE_REPO_NAME}:${IMAGE_TAG}"

#### Step 4

to provision ec2 instance called jenkins-server using terraform

 $ cd infra

 $ ls

<img width="472" height="604" alt="Image" src="https://github.com/user-attachments/assets/30b0a3c4-bbce-46c7-9371-db77f14027b4" />

 #### Step 5

 To run terraform commands

  $ terraform init

  $ terraform fmt

  $ terraform plan
  
  $ terraform apply --auto-approve





