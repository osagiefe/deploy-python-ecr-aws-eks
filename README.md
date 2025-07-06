
### Project Introduction

This is a project that builds a python docker image using dockerfile and docker installed on the jenkins server. This docker image was pushed into elastic container registry (ECR), using jenkins pipeline,while pushing the containerised docker image  in ECR to aws cloud managed kubernetes cluster(aws-eks).

Initially, i had to provision the jenkins server using terraform (IAC).

### Project overview

1.IAM User Setup: Create an IAM user on AWS with the necessary permissions to facilitate deployment and management activities.

2.Infrastructure as Code (IaC): Use Terraform and AWS CLI to set up the Jenkins server (EC2 instance) on AWS.

3.Installing Jenkins: Through the jenkins-script.sh install jenkins software,docker and aws cli

4.Jenkins Server Configuration: Through the manage jenkins menu, install and configure essential plugins and tools such as Docker, amazon ECR.

5.Jenkins Pipelines: Create Jenkins pipelines for deploying the dockerised application to the EKS cluster.

6.Amazon ECR Repositories: Create private repositories for Docker images on Amazon Elastic Container Registry (ECR).

7.AWS CLI - installed on the jenkins server,enable you to interface with AWS infrastructure.

8.AWS configure - to insert Access-key and Secret-key, including Region and json format.

9.Docker - installed on jenkins server, to enable you build docker images

#### Prerequisites:

Before starting the project, ensure you have the following prerequisites:

-An AWS account with the necessary permissions to create resources.

-Terraform and AWS CLI installed on your local machine.

-Basic familiarity with Kubernetes, Docker, Jenkins, and DevOps principles.

-Vscode - code editor installed locally on your machine.

- GitHub - a repository for your project code

https://github.com/clement2019/deploy-python-ecr-aws-eks.git



### Project Workflow
============
#### STAGE 1
==============
#### step 1: Python Flask application

Created a python application,

Collected all the project dependencies or libraries for the project and stored them inside "requirements.txt"

pip freeze >> requirements.txt

#### Step 2: Testing application lacally

Run and test the project locally , to ensure it is working

i. $ python main.py

ii. localhost:5002

created a dockerfile to build docker image for the project

<img width="928" height="442" alt="Image" src="https://github.com/user-attachments/assets/7d2e4b58-7d21-455c-a9d2-3a8b1a07dd52" />


#### To build the application container locally

$ docker run --name seb-container -dp port 5002:5002 <imagename>:latest

to exec into the container

 $ docker exec seb-container -- sh

 $ exit

#### Step 3: We will install Terraform & AWS CLI to deploy our Jenkins Server(EC2) on AWS.

Install & Configure Terraform and AWS CLI on your local machine to create Jenkins Server on AWS Cloud

Terraform Installation Script

wget -O- https://apt.releases.hashicorp.com/gpg | sudo gpg - dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg

echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/

hashicorp.list

sudo apt update

sudo apt install terraform -y

#### AWSCLI Installation Script

curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o 

"awscliv2.zip"

sudo apt install unzip -y

unzip awscliv2.zip

sudo ./aws/install

#### Configure AWS CLI

Run the below command, and add your keys

aws configure


#### Step 4: Deploy the Jenkins Server(EC2) using Terraform

Clone the Git repository: https://github.com/clement2019/deploy-python-ecr-aws-eks.git

$ cd infra

 $ ls

Do some modifications to the backend.tf file such as changing the bucket name(make sure you have created trhe bucket manually on AWS Cloud).
screenshots backend.tf
####
<img width="472" height="604" alt="Image" src="https://github.com/user-attachments/assets/30b0a3c4-bbce-46c7-9371-db77f14027b4" />

####
 jenkins-server.tf 



 jenkins-script.sh

 
 #### Step 4: Run terraform Commands

 Initialize the backend by running the below command

  $ terraform init

   <img width="1602" height="420" alt="Image" src="https://github.com/user-attachments/assets/32b906a3-3c7c-4482-ac78-397f4fec9d76" />


  $ terraform fmt

  $ terraform validate

  Run the below command to get the blueprint of what kind of AWS services will be created.


  $ terraform plan

  Now, run the below command to create the infrastructure on AWS Cloud.

  $ terraform apply --auto-approve
####
  <img width="1882" height="920" alt="Image" src="https://github.com/user-attachments/assets/845a8abe-9d01-4458-b627-c12c3fe64bc9" />

  <img width="2154" height="1334" alt="Image" src="https://github.com/user-attachments/assets/1c451af4-2830-4d11-a43b-b0b0c7703f39" />

  ####
  Now, connect to your Jenkins-Server by clicking on Connect, and connect to jenkins server using ssh.

  <img width="2708" height="1286" alt="Image" src="https://github.com/user-attachments/assets/142972c8-c8d9-4354-a6a7-59c1a8f96775" />

 We have installed some services such as Jenkins, Docker and AWS CLI.

Let’s validate whether all our installed or not.

jenkins --version

docker --version

docker ps

aws --version

<img width="1470" height="532" alt="Image" src="https://github.com/user-attachments/assets/8bcd0e07-278f-4412-b912-401380817039" />

#### step 5: Configure the Jenkins
Now, we have to configure Jenkins. So, copy the public IP of your Jenkins Server and paste it on your favorite browser with an 8080 port.

<img width="2724" height="364" alt="Image" src="https://github.com/user-attachments/assets/f247fa34-eba4-440a-a570-5a360f327864" />

Now, we logged into our Jenkins server.
screenshots

Click on Install suggested plugins

<img width="1980" height="1308" alt="Image" src="https://github.com/user-attachments/assets/98b47a79-b220-4c28-959f-0566dc2fafbc" />

The plugins will be installed

After installing the plugins, continue and create an account

<img width="2158" height="1346" alt="Image" src="https://github.com/user-attachments/assets/60b22579-3048-47fe-bcdf-4419bae7a866" />

Click on Save and Finish.

The Jenkins Dashboard will look like the below snippet.


<img width="2766" height="1370" alt="Image" src="https://github.com/user-attachments/assets/4b9cf74a-c465-4afb-8c2b-6b25bd637b7d" />

install the plugins

<img width="2722" height="1376" alt="Image" src="https://github.com/user-attachments/assets/1ae4ef85-6c89-4840-8a23-cf3df9f5665d" />