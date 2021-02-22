# Migrate Kubernetes Cluster To Amazon Elastic Kubernetes Service (EKS) 

When it comes to containerized applications, Kubernetes has become an obvious orchestration tool of choice nowadays. If you're learning Kubernetes, and trying to set up a Kubernetes cluster on a local machine use this blog post from AWS. [Running Kubernetes on your laptop](https://aws.amazon.com/blogs/opensource/kubernetes-on-laptop/). 

When it comes to a production environment for Kubernetes cluster, we need to ensure that there is no downtime in our cluster. Wouldn't it be nice if this was handled and we keep our focus on building applications.

That's where Amazon Elastic Kubernetes Service comes to the rescue!

What does Amazon Elastic Kubernetes Service do?

Amazon Elastic Kubernetes Service (EKS) is a managed Kubernetes service that makes it easy for you to run Kubernetes on AWS and on-premises. With Amazon EKS, you can take advantage of all the performance, scale, reliability, and availability of AWS infrastructure, as well as integrations with AWS networking and security services, such as Application Load Balancers for load distribution, Identity Access Manager (IAM) integration with role-based access control (RBAC), and Virtual Private Cloud (VPC) for pod networking.

Deploying containers
Managing the scaling of containers and clusters
Resource balancing containers and clusters
Traffic management for services

## Prerequisites And Considerations
There are Prerequisites and considerations to ensure a successful Amazon EKS migration. They are in relation to Security, Networking, Compute Options, Kubernetes Version and Storage. Follow the [Prerequisites and Considerations](https://aws.amazon.com/blogs/architecture/field-notes-migrating-a-self-managed-kubernetes-cluster-on-ec2-to-amazon-eks/)

## Solution


### Install And Configure AWS CLI
[Install AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html)
[Configure AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html)

### Create VPC for EKS cluster
Creating VPC for EKS cluster is easy. Follow the link below and use the CloudFormation Template
https://docs.aws.amazon.com/eks/latest/userguide/create-public-private-vpc.html#create-vpc

We need at least two subnets in different availability zones to configure your EKS cluster.

Record the VPC Id, Subnets and Security Group, we need this information in the next step.

### Creating EKS Cluster

The easiest way to create an EKS cluster is via the AWS console. However, you can also use eksctl or aws cli.
[Create EKS Cluster](https://docs.aws.amazon.com/eks/latest/userguide/create-cluster.html)

Configure Cluster
![configure cluster](https://github.com/sandyghai/Migrate-Kubernetes-Cluster-To-Amazon-Elastic-Kubernetes-Service/blob/master/eks-image-1.png?raw=true)


When you specify networking for your cluster select Cluster Endpoint access option as Public and private which will allow pods to interact with each other and allow you to add services which will be accessible via the outside world.
![specify networking](https://github.com/sandyghai/Migrate-Kubernetes-Cluster-To-Amazon-Elastic-Kubernetes-Service/blob/master/eks-image-2.png?raw=true)

Configure Logging
![configure logging](https://github.com/sandyghai/Migrate-Kubernetes-Cluster-To-Amazon-Elastic-Kubernetes-Service/blob/master/eks-image-3.png?raw=true)

Review and Create

### Update Kube Config

Create backup of original config cp ~/.kube/config  ~/.kube/config-backup

Cluster provisioning takes several minutes. You can query the status of your cluster with the following command or just check via AWS console.
aws eks --region <region-code> describe-cluster --name kube-cluster-demo --query "cluster.status"

When your cluster status is ACTIVE, you can proceed to run aws cli command to update kubeconfig for your EKS cluster. Specify the AWS region where you have created your cluster.
aws eks --region <region-code> update-kubeconfig --name kube-cluster-demo

## References

[AWS EKS Workshop](https://www.eksworkshop.com/) not only provides you with basics of Kubernetes but also how to run clusters on EKS.
[Running Kubernetes on your laptop](https://aws.amazon.com/blogs/opensource/kubernetes-on-laptop/)  helps you set up your laptop for Kubernetes.
[Prerequisites and Considerations to perform successful migration](https://aws.amazon.com/blogs/architecture/field-notes-migrating-a-self-managed-kubernetes-cluster-on-ec2-to-amazon-eks/)
[Install AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html)
[Configure AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html)
[Creating a VPC for your Amazon EKS cluster](https://docs.aws.amazon.com/eks/latest/userguide/create-public-private-vpc.html#create-vpc)
[Create EKS Cluster](https://docs.aws.amazon.com/eks/latest/userguide/create-cluster.html)
[Staring with Minikube](https://kubernetes.io/docs/tutorials/hello-minikube/)