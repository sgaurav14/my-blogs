---
title: "Understanding AWS RDS: A Beginner's Guide"
datePublished: Thu Dec 14 2023 07:27:07 GMT+0000 (Coordinated Universal Time)
cuid: clq4vnqpd000408lb1gnl2bib
slug: understanding-aws-rds-a-beginners-guide
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1702538751280/97f24c8a-57be-402e-b8b5-6485ae9a2b9f.png
tags: aws, rds, aws-rds, relational-database, connect-rds-from-ec2

---

In today's digital age, data is at the heart of every business operation. Whether you run a small blog or a large enterprise, managing your data efficiently is crucial. This is where AWS RDS (Amazon Relational Database Service) comes into play. In this beginner's guide, we'll explore what AWS RDS is and why it's an essential tool for anyone dealing with data.

## **What is AWS RDS?**

AWS RDS is a fully-managed relational database service provided by Amazon Web Services (AWS). But what does that mean exactly? Let's break it down.

### **Relational Database**

A relational database is a type of database that stores and organizes data in a structured way, using tables with rows and columns. This makes it easy to establish relationships between different pieces of information.

### **Fully-Managed**

The term "fully-managed" is key here. With AWS RDS, you don't need to worry about the nitty-gritty details of setting up, maintaining, and securing a database. AWS handles all the heavy lifting for you, allowing you to focus on using the data rather than managing the infrastructure.

## **Why Use AWS RDS?**

Now that we have a basic understanding of what AWS RDS is, let's explore why it's beneficial for both tech and non-tech individuals.

### **1\. Ease of Use**

AWS RDS makes database management accessible to everyone, regardless of technical expertise. Setting up a database instance is as simple as a few clicks in the AWS Management Console.

### **2\. Automatic Backups and Updates**

Imagine never having to worry about backing up your data or updating your database software. AWS RDS takes care of this automatically, ensuring that your data is secure and your database software is up to date.

### **3\. Scalability**

As your business grows, so does your data. AWS RDS allows you to easily scale your database resources to accommodate increased demand without disrupting your operations.

### **4\. Security**

Data security is a top priority, and AWS RDS provides several layers of security, including encryption, network isolation, and regular security patches. Your data is in safe hands.

## **How to Get Started with AWS RDS**

Let's do a project on this -

### **Project: Setting Up AWS RDS, Connecting with EC2, and Performing Database Operations**

If you're ready to dip your toes into AWS RDS, here's a quick step-by-step guide:

### **1\. Sign Up for AWS**

If you don't have an AWS account, you'll need to sign up. Go to the AWS website, click on "Create an AWS Account," and follow the instructions.

### **2\. Access AWS Management Console**

Once your account is set up, log in to the AWS Management Console.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702471019191/7621cc86-4ed9-4d51-ac7e-11d9e711ba04.png align="center")

### **3\. Navigate to RDS**

In the console, find the "Services" dropdown and select "RDS" under the "Database" section.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702471039256/42955694-bff4-4ff0-af47-5e8d02e766f5.png align="center")

Click on "RDS".

### **4\. Create a Database Instance**

Click the "Create Database" button, and the wizard will guide you through the process. You'll need to choose a database engine, specify settings, and set up security measures.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702471080816/97c88c9c-35d5-458d-8e02-34bcbf596cd0.png align="center")

Click on "Create database".

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702471125069/73d2361b-0d77-4083-a255-41f2ea536395.png align="center")

**Select Database Engine:** MySQL as the Database Engine for RDS.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702471196198/86d5c3bc-fe59-4a81-8236-17371f6933ba.png align="center")

**Configure Database Settings:** In the settings options, name your database and configure credentials in the "Credentials settings" option.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702471268951/b1292ee9-f63d-4b47-bef7-96b30c538495.png align="center")

In the Instance configuration option, choose your preferred instance class; for this project, we are using the default class.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702471342351/e64ac795-24d9-4071-8fd9-f66dca075a45.png align="center")

**Configure Storage:** In the "Storage" section, opt for 20GB of space.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702471370667/543a76e3-36b8-40a1-abcd-7a3f13f7eff2.png align="center")

**Configure Connectivity:** In the connectivity option, leave all options at default, including the choice not to connect any specific EC2 instance with your DB.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702471446967/25ab3ae2-6e23-4d95-b67d-0971adc346f2.png align="center")

**Configure Public Access:** In the "Public access" section, select "NO" to restrict public internet access to your DB.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702471502196/4b094827-c227-4bfa-900d-bf69d1260fa7.png align="center")

In the "Database authentication" option, choose "Password authentication" and keep all options unchanged.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702471590273/0b26dd16-4512-42e3-bdfb-2d4c702ad3d2.png align="center")

Review the estimated monthly costs of your DB instance.

Click on "Create database".

Database creation is in progress; please allow some time for completion.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702471653684/162b2715-141b-499b-b97d-f48319cc5bb7.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702471844025/93d8a163-50d9-42e0-9499-fed0081cbc5e.png align="center")

Congratulations! Our first RDS DB has been created successfully.

### **5\.** Create an EC2 instance to connect to our RDS DB and log in to the EC2 instance.

Log in to your EC2 instance (Ubuntu in our project).

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702472119465/4bd580a5-822e-4f6b-842c-36c7802a1237.png align="center")

Check if the MySQL client package is installed or not.

If not, install the mysql-client package.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702472266494/f21ca7a2-e630-448d-8f40-e72d685274fc.png align="center")

Check after the installation.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702472290903/6c45a479-ed75-46e5-8311-f28460e2dffa.png align="center")

It means the MySQL client is installed, and no DB instance is running locally on my EC2 instance.

### 6\. Create a Role for accessing RDS from EC2

Go to IAM dashbaord.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702472436372/9a282ca3-c15d-40d8-ac0b-a3db85926d47.png align="center")

Click on "Roles".

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702472466897/13462418-f16e-4485-aab7-90c9a85e33c6.png align="center")

Click on "Create role".

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702472497502/e0ae6197-29f2-4b9d-877e-7b61dc50246f.png align="center")

Select the trusted entity as "AWS service" since we are creating a role for AWS service, and in the use case, we are selecting EC2.

Scroll down and click on "Next".

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702472614608/a1e4ab20-4226-4886-9de3-2db04c1510fa.png align="center")

Select the policies for RDS full access and CloudWatch that you want to associate with EC2.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702472733047/557f2cdc-5b16-466d-afa5-ac5040d80feb.png align="center")

Give a name to your role and review it.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702472776010/c3ba0b9f-f76f-4893-a856-5e5e89b1914a.png align="center")

Click on "Create role".

Congratulations, our role has been successfully created!

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702472823363/781e71a1-f7f6-4fbc-bec7-0446ecbe90f2.png align="center")

It's time to assign this role to our EC2 instance.

Go to EC2 and select your instance.

Go to Actions -&gt; Security -&gt; Modify IAM role (Click on this)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702472889409/0314aa9a-93ba-4c96-8c32-47dde97d52ef.png align="center")

Now modify the IAM role for our EC2 instance. Select the IAM role.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702472957557/0f9095bc-e333-43f8-87d9-64398c749c0e.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702472969856/2fc58c96-3882-49b9-baf5-737c0d751bc4.png align="center")

Click on "Update IAM role".

### 7\. Configuring the EC2 instance with our RDS database.

Go to the Amazon RDS dashboard and select "Databases."

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702473073662/bfe33768-f965-4b2f-ad89-34e7bd9c52b0.png align="center")

Our database is in an available state. Click on the database that we created. We will be directed to the following screen:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702473121960/5cb375b7-2fe9-4811-852e-bd90fccd03a5.png align="center")

In the "Connectivity & security" tab, copy the Endpoint value (e.g., [my-database.ciztdzgq76lk.us-east-2.rds.amazonaws.com](http://my-database.ciztdzgq76lk.us-east-2.rds.amazonaws.com)) as this is required to connect to the RDS DB from EC2. Scroll down and look for "Connected compute resources."

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702473188708/cc5841b6-1ff7-402a-b025-cc557917ce79.png align="center")

Click on "Set up EC2 connection".

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702473215758/11f483ce-c744-40ec-8c57-f48e6a2c8ce3.png align="center")

Select your EC2 instance and continue.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702473239924/d93e1f8d-cf47-4c90-95af-24ac7a39d1ba.png align="center")

On the next page, it shows that a private network has been created between the EC2 instance and the RDS database.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702473290104/b94e10d7-6340-4313-ac15-eeda7742865d.png align="center")

Review everything and click on "Set up."

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702473311499/5cdf68a4-1c18-48c9-98ea-317eee3309b9.png align="center")

### 8\. Connect to your DB instance from EC2

Login to the EC2 instance and run the below command -

`mysql -u admin -h` [`my-database.ciztdzgq76lk.us-east-2.rds.amazonaws.com`](http://my-database.ciztdzgq76lk.us-east-2.rds.amazonaws.com) `-P 3306 -p`

In the above command, we are connecting with the MySQL client using the user (-u) 'admin' to the host (-h) with our endpoint value that we copied. The option (-P) is for the port, which is set to 3306, and (-p) is for entering the DB password. Enter your database password when prompted.Hurray!! we successfully connected with our RDS database from our EC2 instance.

Let's list the databases in our RDS database.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702473805081/dbde1751-5c09-4ae7-a05e-5b3e7dfabdaa.png align="center")

Let's create a database named 'aws,' create a table, and perform some insertions.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702473913566/b4be29dd-86c8-478a-bdd3-f3e7f5a8aeb2.png align="center")

There are no entries present in your database. Let's add some entries.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702473998387/5ec41f1b-cbac-4e5e-ab92-e81b776fa8a9.png align="center")

And we have performed some insertions in our table in our database.

Let's perform some deletions as well.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702474056428/cfff7c14-723a-4979-95e4-5b265b11b9f3.png align="left")

Whoa! We have successfully deleted the entries from our table.

We have successfully created an RDS DB, connected it to an EC2 instance, and performed various MySQL operations.

Congratulations! You've just set up a fully-managed relational database with AWS RDS.

## **Conclusion**

AWS RDS is a game-changer for anyone looking to manage data without the hassle of database administration. Its user-friendly interface, automatic features, and robust security make it an ideal choice for beginners and seasoned professionals alike. So, whether you're running a personal blog or a thriving business, AWS RDS has got your data management needs covered.

Happy data managing!