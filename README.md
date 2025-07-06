
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

10.EKS Cluster Deployment: Utilize eksctl commands to create an Amazon EKS cluster, a managed Kubernetes service on AWS.

11.Load Balancer Configuration: Configure AWS Application Load Balancer (ALB) for the EKS cluster.

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
####
<img width="928" height="442" alt="Image" src="https://github.com/user-attachments/assets/7d2e4b58-7d21-455c-a9d2-3a8b1a07dd52" />

####
#### To build the application container locally

$ docker run --name seb-container -dp port 5002:5000 <imagename>:latest

to exec into the container

 $ docker exec seb-container -- sh

 $ exit

#### Step 3: We will install Terraform & AWS CLI to deploy our Jenkins Server(EC2) on AWS.

Install & Configure Terraform and AWS CLI on your local machine to create Jenkins Server on AWS Cloud

#### Terraform Installation Script

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

<img width="1682" height="732" alt="Image" src="https://github.com/user-attachments/assets/21a72920-b1d1-4d46-8886-ae94e3d3db4d" />

 jenkins-script.sh

<img width="1764" height="1282" alt="Image" src="https://github.com/user-attachments/assets/7ec8ea41-2e5f-4f11-ab70-fae4445addf7" />
 
 #### Step 5: Run terraform Commands

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

 We have installed some services such as Jenkins, Docker,eksctl and AWS CLI.

Let’s validate whether all our installed or not.

jenkins --version

docker --version

docker ps

aws --version

eksctl version

kubectl version --client

<img width="1470" height="532" alt="Image" src="https://github.com/user-attachments/assets/8bcd0e07-278f-4412-b912-401380817039" />

 #### Step 6:Now install aws eks cluster using eksctl

#### Before running your piplene make sure you install the aws eks kubernetes cluster ruuning the command below

#### Create EKS cluster

  eksctl create cluster --name eks-cluster-204 --node-type t2.small --nodes 1 
  
  --nodes-min 1 --nodes-max 2 --region eu-west-2

  <img width="1930" height="944" alt="Image" src="https://github.com/user-attachments/assets/ed04e90c-ac10-4813-967c-ad9d025e7f30" />

#### Get EKS Cluster service

eksctl get cluster --name eks-cluster-204 --region eu-west-2


<img width="2426" height="642" alt="Image" src="https://github.com/user-attachments/assets/f4fcd9a6-f9d2-4912-905b-256391f0388b" />

#### Get EKS Pod data.

kubectl get pods --all-namespaces

#### Delete EKS cluster

eksctl delete cluster --name eks-cluster-204 --region eu-west-2

#### Step 7:Now we need to jenkins ability to integrate into aws infrastructure

confirm aws cli installation

 aws --version

 $ aws configure

 AWS Access Key [None]: 

 AWS Secret Access Key [None]: 

 Default region name [None]: eu-west-2

Default output format [None]: json

#### Confirn authentication into aws cloud

ubuntu@ip-10-0-10-55:~$ aws s3 ls

ubuntu@ip-10-0-10-55:~$ aws sts get-caller-identity

#### Create IAM role with adminaccess and update IAm role of the the ec2 instance (housing the jenkins)


<img width="2548" height="810" alt="Image" src="https://github.com/user-attachments/assets/bb957aaa-0e42-42e8-a8fe-3b742951ad93" />

#### Create a ECR repository in aws cloud name it 

ecr-repoimg1


#### step8: Create Jenkinsfile and push code to Github

pipeline {
    agent any
    environment {
        AWS_ACCOUNT_ID="***************"
        AWS_DEFAULT_REGION="eu-west-2"
        IMAGE_REPO_NAME="ecr-repoimg1"
        IMAGE_TAG="v1"
        REPOSITORY_URI = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}"
    }
   
    stages {
        
         stage('Logging into AWS ECR') {
            steps {
                script {
                sh "aws ecr get-login-password --region ${AWS_DEFAULT_REGION} | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com"
                }
                 
            }
        }
        
        stage("GitHub checkout") {
            steps {
                script {
 
                    git branch: 'master', url: 'https://github.com/clement2019/deploy-python-ecr-aws-eks.git' 
                }
            }
        }
  
    // Building Docker images
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build "${IMAGE_REPO_NAME}:${IMAGE_TAG}"
        }
      }
    }
   
    // Uploading Docker images into AWS ECR
    stage('Pushing to ECR') {
     steps{  
         script {
                sh "docker tag ${IMAGE_REPO_NAME}:${IMAGE_TAG} ${REPOSITORY_URI}:$IMAGE_TAG"
                sh "docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}:${IMAGE_TAG}"
         }
        }
      }
       stage('Deploying python app to Kubernetes') {
          steps {
            script {
              dir('kubernetes') {
              sh ('aws eks update-kubeconfig --name eks-cluster-204 --region eu-west-2')
              sh 'kubectl config current-context'
              sh "kubectl get ns"
              sh "kubectl apply -f deployment.yaml"
              sh "kubectl apply -f service.yaml"
        }
      }
    }
    }
    }
}

 #### Step 9: Push application code to Github
Go to the application root directory in vscode and run the following commands





Configure the Jenkins
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

install the docker and amazon ecr plugins

<img width="2722" height="1376" alt="Image" src="https://github.com/user-attachments/assets/1ae4ef85-6c89-4840-8a23-cf3df9f5665d" />

<img width="2520" height="1398" alt="Image" src="https://github.com/user-attachments/assets/574302cc-381b-4a00-aed1-4f7b3665e4fd" />

#### create new project in jenkins