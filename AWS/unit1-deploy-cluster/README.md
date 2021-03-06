# Hands-on MarkLogic in the Cloud Workshop (AWS) Unit 1

Table of Contents:  
- [Unit 1 - Create the MarkLogic Cluster](#unit1)  
- [Process to Create a MarkLogic Cluster using CloudFormation Templates](#process)  
- [Enable a MarkLogic AMI](#ami)  
- [Launch your cluster using a CloudFormation template.](#launch)  
- [Check the Status of the New Instance](#instance)  
- [Check the Status of the New Cluster](#cluster)  
- [Access the Cluster](#access)  
- [Optional Exercise](#optional)  

<a name="unit1"></a>
## Unit 1 - Create the MarkLogic Cluster

In unit 1, we will create a three node MarkLogic cluster using CloudFormation templates on Amazon Web Services. For our purposes, a **node** is an Amazon EC2 server instance running MarkLogic. A **cluster** is one or more MarkLogic nodes working together.

It's simple to create a MarkLogic cluster in Amazon Web Services. But before you do, you should become familiar with the process. It's highly recommended to read through the [MarkLogic Server on Amazon EC2 Guide](https://docs.marklogic.com/guide/ec2) .

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

<a name="process"></a>
### Process to Create a MarkLogic Cluster using CloudFormation Templates (ALREADY COMPLETED)

Before starting, make sure you have done the following.  

* You have created an IAM Role to use for your MarkLogic cluster.
* You have created an EC2 Key-Pair to securely shell into your MarkLogic cluster.

Let's begin.

<a name="ami"></a>
### Enable a MarkLogic AMI (ALREADY COMPLETED)

1. Go to <https://aws.amazon.com/marketplace> .
2. In the top search bar, leave the dropdown at the default setting of **AMI & SaaS** then enter the search term `marklogic`.
3. Click the Search icon or press the ENTER key to begin the search.
4. In the results, click on **MarkLogic Developer 9**.
  ![](images/marketplace.png)
5. Click the **Continue to Subscribe** button.
6. Click the **Service Catalog** tab.
7. Click the **Accept Software Terms** button.
  ![](images/acceptTerm.png)
8. You've now subscribed to the MarkLogic AMI and can proceed to use a CloudFormation template to create your running instance.

<a name="launch"></a>
### Launch your cluster using a CloudFormation template

0. Log in via the link provided and with the your credentials.
1. Go to [CloudFormation page](https://us-west-2.console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks?filter=active) of AWS Web Console. Change your region at the top right corner to **Oregan**. For the purpose of this excercise, we are using this region (US West 2).
![](images/cloudformation.png)
2. Click the **Create stack** button.
3. In the **Select Template** section, select the radio button "Specify an Amazon S3 template URL". Copy and paste the URL to the `mlcluster-vpc.template` provided. <a name="step3"></a>
![](images/selectTemplate.png)
4. Click the **Next** button.
![](images/stackParameters.png)
5. Fill in the following information. <a name="step5"></a>
	* Stack Name - a **unique** stack name across the account (example: your full name). Take a note of this stack name. You will need it later for tags.
	* IAM Role - a previously created Identity and Access Management role name. **Use the value provided**.
	* Volume Size - leave at the default of 10 GB.
	* Volume Type - leave at the default.
	* Instance Type - Use **m3.medium** for this exercise.
	* Spot Price - leave at the default.
	* SSH Key Name - a previously created EC2 Key-Pair name. **Use the value provided**.
	* Number of Zones - leave at the default of 3.
	* Nodes Per Zone - total number of nodes per Zone. Leave this at 1 to create a simple 3-node cluster.
	* Availability Zone - select **us-west-2a**, **us-west-2b** and **us-west-2c** from dropdown menu.
	* Instance Public IP - leave at the default of disable.
	* Logging SNS ARN - optional. Leave at the default value.
	* VPC CIDR - leave at the default value.
	* Subnet 1 CIDR - leave at the default value.
	* Subnet 2 CIDR - leave at the default value.
	* Subnet 3 CIDR - leave at the default value.
	* Admin User -  the MarkLogic administrator user name. The user and password will be created when the cluster is created. We will use `mlwadmin` for the administrator name.
	* Admin Password - the MarkLogic administrator user password. The user and password will be created when the cluster is created. We will use `MarkLogicWorld_2018` as the password.
	* Licensee - leave at the default value "none" to use the included Developer's License.
	* LicenseKey - leave at the default value "none"
6. Click the **Next** button. <a name="step6"></a>
7. On the **Options** page, create a tag with key as `Name` and value as your stack name specified in the previous page. Leave all other settings to the defaults, scroll to the bottom and click the **Next** button.
![](images/tagStack.png)
8. On the **Review** page, scroll to the bottom and check the box "I acknowledge that AWS CloudFormation might create IAM resources with custom names". This is to authorize CloudFormation to create IAM role based on the permissions specified in the template. Then click the **Create** button.
![](images/ackIAM.png)
9. You are returned to the **Create Stack** page while your CloudFormation stack is being created.
10. After the stack is created, the page will refresh with the name of your completed stack. You should expect to see "CREATE COMPLETE" of your stack. This process may take up to 10 minutes.
![](images/createComplete.png)
11. Scroll down to find the **Outputs** tab, click it.
12. The URL to access MarkLogic's Administrative Interface page on port 8001 is in the `Value` column. Note this URL to use later.
![](images/stackOutputs.png)

<a name="instance"></a>
## Check the Status of the New Instance

1. Open another tab in your browser.
2. Go to the AWS Console page at <https://console.aws.amazon.com> .
3. In the **Compute** section, click on **EC2**.
4. On the left side, click on **Instances** in the **Instances** group.
![](images/instanceStatus.png)
5. In the list of EC2 instances, make sure the state is `started` in the **Instance State** column and the **Status Checks** column is `2/2 checks passed` with a green checkmark icon.
6. If the status passes both checks, the instance has started successfully.

<a name="cluster"></a>
## Check the Status of the New Cluster

> Note: it takes approximately 10 to 15 minutes for the created MarkLogic server instances to pass their validation checks and initialize MarkLogic clusters.

1. Go to CloudFormation page at <https://us-west-2.console.aws.amazon.com/cloudformation/home>
2. Select the stack you just created. Note that you should select the parent stack, not those substacks with a "NESTED" label.
3. Click on the **Resource** tab at the bottom.
4. From the list of resources, find the ElasticLoadBalancer. Most likely, it's on top.
5. Click on the **Physical ID** of ElasticLoadBalancer. You will be redirected to ElasticLoadBalancer page.
![](images/elb.png)
6. Click on the **Instances** tab at the bottom of Load Balancer page.
7. You should see three instances of status "In Service" when the MarkLogic cluster is up. If not yet, wait for a few minutes and refresh.
![](images/clusterStatus.png)

<a name="access"></a>
## Access the Cluster

Use the URL in the CloudFormation stack's **Outputs** tab to navigate to MarkLogic's Admin Interface.  
![](images/adminGui.png)

If you need to copy the URL again: 

* Navigate to the **CloudFormation** service by clicking on the **Services** link at the top of the AWS Console page. 
* Select your CloudFormation stack by clicking the checkbox. 
* Click the **Outputs** tab below the list of CloudFormation stacks for the URL.

<a name="optional"></a>
### Optional Exercise

Once you go through the basic flow of deploying a cluster, you can also customize the cluster by using different deployment type.

- Use the CloudFormation template that deploys cluster into **existing VPC**. Copy and paste the URL to the `mlcluster.template` provided. ([Step 3 of Launching](#step3))
- Fill in the following parameters. ([Step 5 of Launching](#step5))
	* Stack Name - a **unique** stack name across the account (example: your full name). Take a note of this stack name. You will need it later for tags.
	* IAM Role - a previously created Identity and Access Management role name. **Use the value provided**.
	* Volume Size - leave at the default of 10 GB.
	* Volume Type - leave at the default.
	* Instance Type - Use **m3.medium** for this exercise.
	* Spot Price - leave at the default.
	* SSH Key Name - a previously created EC2 Key-Pair name. **Use the value provided**.
	* **Number of Zones - Set to 1. Deploy to only 1 availability zone.**
	* Nodes Per Zone - total number of nodes per Zone. Leave this at 1 to create a simple 3-node cluster.
	* **Availability Zone - select "us-west-2a" from dropdown menu.**
	* Instance Public IP - leave at the default of disable.
	* Logging SNS ARN - optional. Leave at the default value.
	* **VPC - Select the VPC you created in this exercise unit.**
	* **Subnets - Select one subnet you created in this excercise unit.**
	* Admin User -  the MarkLogic administrator user name. The user and password will be created when the cluster is created. We will use `mlwadmin` for the administrator name.
	* Admin Password - the MarkLogic administrator user password. The user and password will be created when the cluster is created. We will use `MarkLogicWorld_2018` as the password.
	* Licensee - leave at the default value "none" to use the included Developer's License.
	* LicenseKey - leave at the default value "none"

Now you should see a new one node cluster is coming up!
