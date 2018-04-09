# Hands-on MarkLogic in the Cloud Workshop (Azure) Unit 1

Table of Contents:
- ![Unit 1 - Create the MarkLogic Cluster](unit-1-create-the-marklogic-cluster)
	- ![Process to Create a MarkLogic Cluster using CloudFormation Templates](Process-to-Create-a-MarkLogic-Cluster-using-CloudFormation-Templates)
	- ![Enable a MarkLogic AMI](Enable-a-MarkLogic-AMI)
	- ![Launch your cluster using a CloudFormation tempate.](Launch-your-cluster-using-a-CloudFormation-tempate)
	- ![Check the Status of the New Instance](Check-the-Status-of-the-New-Instance)
	- ![Access the Cluster](Access-the-Cluster)

## Unit 1 - Create the MarkLogic Cluster

In unit 1, we will create a three node MarkLogic cluster using Solution Templates on Microsoft Azure. For our purposes, a **node** is an Azure VM instance running MarkLogic. A **cluster** is one or more MarkLogic nodes working together.

It's simple to create a MarkLogic cluster in Amazon Web Services. But before you do, you should become familiar with the process. It's highly recommended to read through the [MarkLogic Server on Microsoft® Azure® Guide](http://docs.marklogic.com/guide/azure) .

An overview of the procedures follows with links to MarkLogic documentation.

Procedure                         | For Details See...
--------------------------------- | -----------------------
If you don’t already have an Amazon EC2 account, create one. | [Creating an Amazon EC2 Account](https://docs.marklogic.com/guide/ec2/GettingStarted#id_52961) 
Enable a MarkLogic Server AMI. | [Enabling a MarkLogic Server for EC2 AMI](https://docs.marklogic.com/guide/ec2/GettingStarted#id_99793)
Open the Amazon AWS Management Console. | [Accessing the AWS Management Console](https://docs.marklogic.com/guide/ec2/GettingStarted#id_69008)
Create an IAM role. | [Creating an IAM Role](https://docs.marklogic.com/guide/ec2/GettingStarted#id_39710)
If you don’t already have a key pair, create one. | [Creating a Key Pair](https://docs.marklogic.com/guide/ec2/GettingStarted#id_24571)
Create a Simple Notification Service (SNS) Topic. | [Creating a Simple Notification Service (SNS) Topic](https://docs.marklogic.com/guide/ec2/GettingStarted#id_69696)
Create CloudFormation stack from a CloudFormation template. | [Deploying MarkLogic on EC2 Using CloudFormation](https://docs.marklogic.com/guide/ec2/CloudFormation)
Open the MarkLogic Server Admin interface. | [Accessing a MarkLogic Server Instance](https://docs.marklogic.com/guide/ec2/managing#id_18558)

