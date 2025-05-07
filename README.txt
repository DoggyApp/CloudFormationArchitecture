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

🧱 Architecture

    EKS Cluster

        Control plane managed by AWS

        Worker nodes in private subnets across 2 AZs

    VPC

        Custom VPC with public, private and protected subnets

        NAT Gateway setup

    Bastion Host

        EC2 instance in a public subnet

        Used for secure management access via SSM 

🚀 Deployment

    Ensure you have AWS CLI and CloudFormation permissions configured.

        git clone https://github.com/DoggyApp/CloudFormationArchitecture.git
        cd CloudFormationArchitecture
    
