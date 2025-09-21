# Before running your pipline make sure you install the aws eks kubernetes cluster runing the command below

# Create EKS cluster
  eksctl create cluster --name eks-cluster-100 --node-type t3.medium --nodes 2 --nodes-min 2 --nodes-max 3 --region us-east-1

## Get EKS Cluster service
eksctl get cluster --name eks-cluster-100 --region us-east-1

## Get EKS Pod data.
kubectl get pods --all-namespaces

## Delete EKS cluster
eksctl delete cluster --name eks-cluster-100 --region us-east-1