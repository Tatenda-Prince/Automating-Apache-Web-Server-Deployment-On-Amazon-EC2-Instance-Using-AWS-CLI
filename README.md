# Automating-Apache-Web-Server-Deployment-On-Amazon-EC2-Instance-Using-AWS-CLI

![image alt](https://github.com/Tatenda-Prince/Automating-Apache-Web-Server-Deployment-On-Amazon-EC2-Instance-Using-AWS-CLI/blob/50171ba330c62d2a8f80eaad5a9ddb9db8e80717/Images/Screenshot%202024-12-23%20122242.png)

# Intro 

Today, we’re going to leverage the practical use of the AWS CLI to increase efficiency and automate steps to deploying an Apache Web Server on an Amazon EC2 Instance. After we complete our “Saving Time with AWS CLI” task.

# Background

As a Cloud Engineer, part of your main task is to help your organization operate with greater efficiency. The use of the AWS Management Console will also get the job done but there is much time wasted clicking through the console. To increase efficiency and realize the benefits of automating processes, using the AWS CLI is usually the better option.

# Use Case

You’ve just been hired as a Cloud Engineer for the Up The ChELS organization. You notice the current engineers regularly use the AWS Management Console to accomplish tasks managing AWS resources. You also realize that they tend to run repetitive tasks, involving Linux commands, one at a time in the command line. You want to show them a more efficient way to automate daily processes like launching an Amazon EC2 web server. You decide to practice the steps yourself first before showing them how they can also.

# Prerequisites

An AWS Account and IAM User

Basic working knowledge in the AWS Management Console

Basic Linux command line knowledge in your preferred Linux CLI

sudo privileges on Linux system as non-root user

Basic working knowledge of Vim

# Step 1: Retrieve and create resources to launch EC2

Before we can launch a new Amazon EC2 Instance, we have to retrieve specific resource information from our AWS account which we will use to configure our EC2 Instance. We will also create some resources before we can retrieve further information. The resources needed for our task are listed below —

VPC ID — ID of virtual network dedicated to your AWS account

AMI ID — ID for specific template that contains software configuration to launch or EC2 Instance

Security Group ID — ID for Firewall that controls ingress and egress network traffic to EC2 Instance

Key Pair Name — name of key pair used to give us programmatic access to our AWS resources

# Retrieve VPC ID

To retrieve our VPC ID, run the following command —

aws ec2 describe-vpcs 

Your screen should display the current configurations of your VPCs. Locate and note the “VpcId” of the VPC where you will launch the EC2 Instance, as seen below.

![image alt](https://github.com/Tatenda-Prince/Automating-Apache-Web-Server-Deployment-On-Amazon-EC2-Instance-Using-AWS-CLI/blob/8d244c3c8c937c59c2387f9622c9996d5449378a/Images/Screenshot%202024-12-23%20153836.png)

# Create new Security Group in VPC

We need to create a new Security Group in our VPC to use with our EC2 Instance to have control of the network traffic flowing in and out of it. To successfully run this command, we’ll need to provide the information of the group name, group description and VPC ID retrieved from our previous command.

To create a new Security group, run the following command —

aws ec2 create-security-group --group-name [group_name] --description "[group_description]"
--vpc-id [vpc_id]

Copy and save the Security Group ID displayed in your Linux CLI after running the command, as seen below.

![image alt](https://github.com/Tatenda-Prince/Automating-Apache-Web-Server-Deployment-On-Amazon-EC2-Instance-Using-AWS-CLI/blob/6579c14cf621c8c58021e0c021473a50089d1122/Images/Screenshot%202024-12-23%20154204.png)


You can also verify that the Security Group was created by heading over to the AWS Management Console. As seen below, the new Security Group named “proj4cli” has been created.

![image alt](https://github.com/Tatenda-Prince/Automating-Apache-Web-Server-Deployment-On-Amazon-EC2-Instance-Using-AWS-CLI/blob/837d5bb438afb38af4994eda371c8cc0a311c2ce/Images/Screenshot%202024-12-23%20154318.png)


# Configure inbound (ingress) Firewall rules to the Security Group


We need to allow “ssh” traffic on port 22, so we have the capability to securely connect into our EC2 Instance to perform tasks. We also need to allow “http” traffic on port 80, to allow us to connect to the EC2 instance over the internet on our browser and request/be served data to view our Website.

To allow “ssh” on port 22, run the following command —

aws ec2 authorize-security-group-ingress --group-id [security_group_id] --protocol tcp --port 22 --cidr 0.0.0.0/0

![image alt](https://github.com/Tatenda-Prince/Automating-Apache-Web-Server-Deployment-On-Amazon-EC2-Instance-Using-AWS-CLI/blob/98aab621957e215030a8787a86cb9a17d9e486e2/Images/Screenshot%202024-12-23%20154541.png)


To allow “http” on port 80, run the following command —

aws ec2 authorize-security-group-ingress --group-id [security_group_id] --protocol tcp --port 80 --cidr 0.0.0.0/0


![image alt](https://github.com/Tatenda-Prince/Automating-Apache-Web-Server-Deployment-On-Amazon-EC2-Instance-Using-AWS-CLI/blob/ea02dd0d012815c6491407c1a869f58f7bcd48b8/Images/Screenshot%202024-12-23%20154800.png)


Verify that the ports are configured correctly by navigating to your VPCs Security Groups Inbound rules in the AWS Management Console. You should see the newly configured open ports, as seen below.

![image alt](https://github.com/Tatenda-Prince/Automating-Apache-Web-Server-Deployment-On-Amazon-EC2-Instance-Using-AWS-CLI/blob/c0f25c08c17175b64dd901da6d67783c6dd83c04/Images/Screenshot%202024-12-23%20154846.png)

# Create an SSH Key Pair

aws ec2 create-key-pair --key-name [group_name]


![image alt](https://github.com/Tatenda-Prince/Automating-Apache-Web-Server-Deployment-On-Amazon-EC2-Instance-Using-AWS-CLI/blob/373b8732f5f34ab8be5942e48d4f0be07a1c4bac/Images/Screenshot%202024-12-23%20155026.png)


Verify that the key pair has been created by running the following command to view it —


![image alt]()


# 












