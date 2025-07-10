📌 Overview

This project provides an infrastructure-as-code solution for deploying a highly available 
Amazon EKS (Elastic Kubernetes Service) cluster across two Availability Zones using AWS 
CloudFormation. The setup includes an EC2 Bastion Host to provide secure SSH access for 
managing the cluster and related resources.

🛠️ Features

    🏗️ Deploys a fully managed EKS cluster on AWS

    🌐 Spans across two Availability Zones for high availability

    🔐 EC2 Bastion Host for secure management access

    🔄 Supports cluster upgrades and scalability

    🧩 Modular CloudFormation templates for reusability

    🏃 Automated deployment of Kubernets cluster, monitoring namespace, alerting, ingress 
    and application for complete end to end automation. 

🧱 Architecture

    EKS Cluster

        Control plane managed by AWS

        Worker nodes in private subnets across 2 AZs

        2 Node groups, one for monitoring and the other for the application and core Kube 
            functions 

    VPC

        Custom VPC with public, private and protected subnets

        NAT Gateway setup

    Bastion Host

        EC2 instance in a private subnet

        Used for secure management access via SSM 

🚀 Deployment

    Ensure you have AWS CLI and CloudFormation permissions configured.

        git clone https://github.com/DoggyApp/CloudFormationArchitecture.git
        cd CloudFormationArchitecture

        aws cloudformation package --template-file master.yaml --s3-bucket doggy-template-build-terminal --output-template-file packaged-root-template.yaml
        aws cloudformation validate-template --template-body file://packaged-root-template.yaml
        aws cloudformation deploy \
          --template-file packaged-root-template.yaml \
          --stack-name doggy-stack \
          --capabilities CAPABILITY_NAMED_IAM \
          --region us-east-1 \ #i accidentaly baked in the region in the code, it would be a furture project to abstract it 
          --parameter-overrides \ # go to IAM> users> <YourUser> >Security Credentials> Access Keys you can create a new one or maybe use an existing one 
            AdminAccessKey=<access_key> \ # this key will be placed in the startup script in the ec2.yaml file and automatically configure your credentials to access the kubernetes cluster.
            AdminSecretKey=<secret_access_key> \ #  As long as you issue these commands with the same user as the access key and that user has permissions use EKS, you should be able to access the cluster with the below command 
            email=<email> \# use the email you would like to recieve alerts to 
            AwsId=<AwsId> \
            OpenAiARN=<SecretARN> 

        # a kubernetes cluster is only accessible by the user OR RESOURCE that deployed it 
        # once the cluster is deployed go to EC2 > Instances > mgmtInstance > connect > SSM > Connect 

        sudo su - ec2-user # when you log in from SSM you are automatically set as ssm-user change to ec2-user for higher priviledges   

🛡️ Security 

    🔐 Private subnets are used to restrict access to the Kubernetes cluster and worker nodes 
    are placed in the private subet. 

    🔐 Security group on mgmt instance blocks all traffic, only allowing SSM, and is placed in
    a private subnet to ensure no public access. 

    🔐 Security group on Kubernetes cluster blocks all traffic, except TCP originating from mgmt 
    instance security group. 

    🔐 Mgmt instance role only given access to doggy-app-cluster resource. 

    🔐 Access Key and Secret Access Key (needed to access kubernetes cluster) stored as secrets 
    and pulled from AWS secrets manager during build. 

    🔐 KMS used to encrypt block storage on mgmt instance. 

    🔐 AWS profile uses admin user, not root, all users are locked behind MFA. 

    🔐 Only admin user has permission to assume role of the pipeline and access Kube Cluster. 




    
