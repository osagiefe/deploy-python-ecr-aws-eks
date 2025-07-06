pipeline {
    agent any
    environment {
        AWS_ACCOUNT_ID="759623136685"
        AWS_DEFAULT_REGION="eu-west-2"
        IMAGE_REPO_NAME="ecr-repoimg1"
        IMAGE_TAG="1.0"
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



