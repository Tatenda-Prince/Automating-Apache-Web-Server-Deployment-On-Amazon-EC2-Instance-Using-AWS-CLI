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

![image alt]()







