# Automated Apache Web Server Deployment on EC2 with AWS CLI

"Saving Time with AWS CLI" 

![image alt](https://github.com/Tatenda-Prince/Automating-Apache-Web-Server-Deployment-On-Amazon-EC2-Instance-Using-AWS-CLI/blob/50171ba330c62d2a8f80eaad5a9ddb9db8e80717/Images/Screenshot%202024-12-23%20122242.png)

## Intro 

Today, we’re going to leverage the practical use of the AWS CLI to increase efficiency and automate steps to deploying an Apache Web Server on an Amazon EC2 Instance. After we complete our “Saving Time with AWS CLI” task.

## Background

As a Cloud Engineer, part of your main task is to help your organization operate with greater efficiency. The use of the AWS Management Console will also get the job done but there is much time wasted clicking through the console. To increase efficiency and realize the benefits of automating processes, using the AWS CLI is usually the better option.

## Use Case

You’ve just been hired as a Cloud Engineer for the Up The Chels organization. You notice the current engineers regularly use the AWS Management Console to accomplish tasks managing AWS resources. You also realize that they tend to run repetitive tasks, involving Linux commands, one at a time in the command line. You want to show them a more efficient way to automate daily processes like launching an Amazon EC2 web server. You decide to practice the steps yourself first before showing them how they can also.

## Prerequisites

1.An AWS Account and IAM User

2.Basic working knowledge in the AWS Management Console

3.Basic Linux command line knowledge in your preferred Linux CLI

4.sudo privileges on Linux system as non-root user

5.Basic working knowledge of Vim

## Step 1: Retrieve and create resources to launch EC2

Before we can launch a new Amazon EC2 Instance, we have to retrieve specific resource information from our AWS account which we will use to configure our EC2 Instance. We will also create some resources before we can retrieve further information. The resources needed for our task are listed below —

1.VPC ID — ID of virtual network dedicated to your AWS account

2.AMI ID — ID for specific template that contains software configuration to launch or EC2 Instance

3.Security Group ID — ID for Firewall that controls ingress and egress network traffic to EC2 Instance

4.Key Pair Name — name of key pair used to give us programmatic access to our AWS resources

## Retrieve VPC ID

To retrieve our VPC ID, run the following command —
```language
aws ec2 describe-vpcs 
```
Your screen should display the current configurations of your VPCs. Locate and note the “VpcId” of the VPC where you will launch the EC2 Instance, as seen below.

![image alt](https://github.com/Tatenda-Prince/Automating-Apache-Web-Server-Deployment-On-Amazon-EC2-Instance-Using-AWS-CLI/blob/8d244c3c8c937c59c2387f9622c9996d5449378a/Images/Screenshot%202024-12-23%20153836.png)

## Create new Security Group in VPC

We need to create a new Security Group in our VPC to use with our EC2 Instance to have control of the network traffic flowing in and out of it. To successfully run this command, we’ll need to provide the information of the group name, group description and VPC ID retrieved from our previous command.

To create a new Security group, run the following command —

```language
aws ec2 create-security-group --group-name [group_name] --description "[group_description]"
--vpc-id [vpc_id]
```

Copy and save the Security Group ID displayed in your Linux CLI after running the command, as seen below.

![image alt](https://github.com/Tatenda-Prince/Automating-Apache-Web-Server-Deployment-On-Amazon-EC2-Instance-Using-AWS-CLI/blob/6579c14cf621c8c58021e0c021473a50089d1122/Images/Screenshot%202024-12-23%20154204.png)


You can also verify that the Security Group was created by heading over to the AWS Management Console. As seen below, the new Security Group named “tatenda4cli” has been created.

![image alt](https://github.com/Tatenda-Prince/Automating-Apache-Web-Server-Deployment-On-Amazon-EC2-Instance-Using-AWS-CLI/blob/837d5bb438afb38af4994eda371c8cc0a311c2ce/Images/Screenshot%202024-12-23%20154318.png)


## Configure inbound (ingress) Firewall rules to the Security Group


We need to allow “ssh” traffic on port 22, so we have the capability to securely connect into our EC2 Instance to perform tasks. We also need to allow “http” traffic on port 80, to allow us to connect to the EC2 instance over the internet on our browser and request/be served data to view our Website.

To allow “ssh” on port 22, run the following command —


```language
aws ec2 authorize-security-group-ingress --group-id [security_group_id] --protocol tcp --port 22 --cidr 0.0.0.0/0
```


![image alt](https://github.com/Tatenda-Prince/Automating-Apache-Web-Server-Deployment-On-Amazon-EC2-Instance-Using-AWS-CLI/blob/98aab621957e215030a8787a86cb9a17d9e486e2/Images/Screenshot%202024-12-23%20154541.png)



To allow “http” on port 80, run the following command —


```language
aws ec2 authorize-security-group-ingress --group-id [security_group_id] --protocol tcp --port 80 --cidr 0.0.0.0/0
```


![image alt](https://github.com/Tatenda-Prince/Automating-Apache-Web-Server-Deployment-On-Amazon-EC2-Instance-Using-AWS-CLI/blob/ea02dd0d012815c6491407c1a869f58f7bcd48b8/Images/Screenshot%202024-12-23%20154800.png)



Verify that the ports are configured correctly by navigating to your VPCs Security Groups Inbound rules in the AWS Management Console. You should see the newly configured open ports, as seen below.


![image alt](https://github.com/Tatenda-Prince/Automating-Apache-Web-Server-Deployment-On-Amazon-EC2-Instance-Using-AWS-CLI/blob/c0f25c08c17175b64dd901da6d67783c6dd83c04/Images/Screenshot%202024-12-23%20154846.png)


## Create an SSH Key Pair

```language
aws ec2 create-key-pair --key-name [group_name]
```


![image alt](https://github.com/Tatenda-Prince/Automating-Apache-Web-Server-Deployment-On-Amazon-EC2-Instance-Using-AWS-CLI/blob/373b8732f5f34ab8be5942e48d4f0be07a1c4bac/Images/Screenshot%202024-12-23%20155026.png)


Verify that the key pair has been created by running the following command to view it —

```language
aws ec2 describe-key-pairs --key-name [group_name]
```


![image alt](https://github.com/Tatenda-Prince/Automating-Apache-Web-Server-Deployment-On-Amazon-EC2-Instance-Using-AWS-CLI/blob/ed0a1dea48c1ad37c9a7a5ffc9c34bbdc87196de/Images/Screenshot%202024-12-23%20155032.png)


## Get Amazon Machine Image (AMI) ID

We will use the Amazon Linux 2 AMI which is part of the AWS free tier. To locate the AMI ID, navigate to the EC2 Instance dashboard in the AWS Management Console, click on “AMI Catalog” then copy and save the AMI ID of the Amazon Linux 2 AMI, as seen below.

![image alt](https://github.com/Tatenda-Prince/Automating-Apache-Web-Server-Deployment-On-Amazon-EC2-Instance-Using-AWS-CLI/blob/8edf7ce94e1feb3314ddd153e72858f51160226a/Images/Screenshot%202024-12-24%20191506.png)

We’ve now collected all the resource information needed to launch the configured EC2 Instance from the AWS CLI, namely our VPC ID, AMI ID Security Group ID and Key Pair Name.

Now let’s progress onto step 2.

## Step 2: Create script for bootstrapping EC2 Instance

We need to install an Apache Web Server on our EC2 Instance to enable it to serve HTTP content over the internet to our browsers. To accomplish this, we have to create a bash script to install, enable and start an Apache Web Server through bootstrapping the EC2 Instance. Bootstrapping refers to the process of adding scripts to an EC2 Instance’s user data to be executed when the instance starts.

```language
mkdir [directory_name]

 cd [directory_name]
```

## Create bash script

```language 
sudo vim [script_name.sh]
```


![image alt](https://github.com/Tatenda-Prince/Automating-Apache-Web-Server-Deployment-On-Amazon-EC2-Instance-Using-AWS-CLI/blob/fe86286a86512100011d4ce0ba74136e5529fbed/Images/Screenshot%202025-01-03%20154639.png)


## Step 3: Launch EC2 Web Server with t2.micro instance type bootstrapped with bash script

Use retrieved and created resource info to launch EC2 Web Server

Refer back to all the retrieved and created resource information from step 2. We will now use them, along with our bash script, to finally launch our EC2 Apache Web Server.

To launch our EC2 Web Server, run the following command —

```language
aws ec2 run-instances --image-id [ami_id] --count 1 --instance-type t2.micro --key-name [key_pair_name] --security-group-ids [security_group_id] --user-data file://[script_name.sh]
```


## Press [Shift:wq!] to exit out of the outputted info screen.

Now verify that the EC2 Instance was created by navigating to the EC2 Instance dashboard in the AWS Management Console. You should see the newly launched EC2 Web Server, as seen below. Give it a few minutes for the instance state to change to “Running”.

![image alt](https://github.com/Tatenda-Prince/Automating-Apache-Web-Server-Deployment-On-Amazon-EC2-Instance-Using-AWS-CLI/blob/172c51c6bd13a2e97953f8cafd1a3f940beeab0b/Images/Screenshot%202025-01-03%20154706.png)


## Step 4: Connect to your EC2 Instance running Apache Web Server through browser

Retrieve the public IP Address of the EC2 Instance from the Amazon EC2 dashboard “Networking” tab, copy and paste it in the address bar of your preferred browser, then hit “enter” on your keyboard. Your browser should display the Apache Web Server default Webpage, as seen below.


![image alt](https://github.com/Tatenda-Prince/Automating-Apache-Web-Server-Deployment-On-Amazon-EC2-Instance-Using-AWS-CLI/blob/c4344e8a3d0716759cae14c15990c9a36af41450/Images/Screenshot%202025-01-03%20154800.png)

## YAY Success!

You just completed the “Saving Time with AWS CLI” task! You can now show your engineer colleagues how to be more efficient and automate an Apache Web Server deployment on an Amazon EC2 Instance using the AWS CLI.

















