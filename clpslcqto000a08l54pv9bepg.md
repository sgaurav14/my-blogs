---
title: "Renting Servers in the Cloud: A Layman's Introduction to AWS EC2"
datePublished: Tue Dec 05 2023 17:05:23 GMT+0000 (Coordinated Universal Time)
cuid: clpslcqto000a08l54pv9bepg
slug: renting-servers-in-the-cloud-a-laymans-introduction-to-aws-ec2
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1701787355778/78e51e05-0574-4047-ab5c-e3318a4f2541.webp
tags: ec2, aws, aws-certified-solutions-architect-associate, awswithtws-7daysofaws

---

AWS EC2, or Amazon Elastic Compute Cloud, is a web service provided by Amazon Web Services (AWS) that allows you to rent virtual servers in the cloud. Let's break down this definition for a non-technical audience:

1. **Amazon Web Services (AWS):** AWS is a cloud computing platform offered by Amazon. Instead of running software or storing data on your local computer or servers, you can use AWS to access computing power, storage, and other resources over the internet.
    
2. **Elastic Compute Cloud (EC2):** EC2 is a specific service within AWS. It's like renting a virtual computer in the cloud. This virtual computer is called an "instance." These instances can be customized to meet your specific needs regarding computing power, memory, and storage.
    
3. **Virtual Servers:** When we say "virtual servers," think of them as computers that exist in the cloud. They don't physically exist; instead, they run on powerful servers maintained by AWS. You can use these virtual servers for various purposes, such as running applications, hosting websites, or processing data.
    
4. **Renting:** Rather than buying and maintaining physical servers, you pay for the computing resources you use. It's like renting a computer for a specific period, and you can scale up or down based on your needs. This flexibility is one of the key advantages of cloud computing.
    

In summary, AWS EC2 is a service that lets you easily and flexibly rent virtual computers in the cloud. This helps businesses and individuals access computing power without the need to invest in and manage physical hardware.

### Deploying an EC2 Instance

1. **Navigate to AWS Console:**
    
    Begin by logging into your AWS account and accessing the AWS Management Console. If you don't have an account, you can easily create one to start exploring the world of cloud computing.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701793609731/7c54d9dd-6ead-466a-b47a-fdbe7bf7ac3c.png align="center")
    
2. **Access EC2 Service:**
    
    Once logged in, find and select the EC2 service. This is where the magic happens – where you'll launch, manage, and monitor your virtual servers.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701793718829/bc302281-c59e-496a-b8ae-869cdabdac93.png align="center")
    
3. **Launch Your Instance:**
    
    Click on the "Launch Instance" button to initiate the instance creation process. Here, you'll be prompted to choose an Amazon Machine Image (AMI), which is essentially the software configuration for your virtual server.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701793823452/7023767a-46b0-459e-9c16-c28c4ad59612.png align="center")
    
    Enter the name of the instance which you want to configure. Also in the AMI section, select the Operating System which you want to run.
    
4. **Select Instance Type:**
    
    Next, select the type of instance you need. AWS offers a variety of instances optimized for different use cases – from general-purpose to compute-optimized and memory-optimized instances.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701793929326/58c5b098-6c63-4eed-9ba5-b3f2cd2d6043.png align="center")
    
    We are selecting "t2.micro" as this is free for ~750 hours in the first year of your new AWS account.
    
5. **Configure Instance:**
    
    Customize your instance by specifying details such as the key pair, number of instances, network settings, and storage requirements. This is where you tailor the virtual environment to meet your specific needs.
    
    **Key pair:** In AWS EC2, a key pair is a set of security credentials that consists of a public key and a private key. This key pair is used to securely connect to and authenticate with your EC2 instances. Let's break down the key concepts:
    
    * **Public Key:**
        
        * The public key is shared and can be freely distributed. It's used by AWS to encrypt data that can only be decrypted with the corresponding private key.
            
        * When you launch an EC2 instance, you specify the key pair to be associated with the instance. The public key is then embedded in the instance, allowing for secure communication.
            
    * **Private Key:**
        
        * The private key is kept secret and should only be known to the owner. It's used to decrypt data that was encrypted with the corresponding public key.
            
        * You must securely store your private key because it is essential for accessing your EC2 instances. Losing the private key means losing access to the instances associated with that key pair.
            
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701794885906/923f8002-b83e-43d1-bb48-87e6db78d5e1.png align="center")
    
    Create a new key pair by providing a name, selecting the key pair type, and choosing the Private key file format. In our example, we are using .pem as the file format.
    
6. **Configure Security Groups:**
    
    Security is a top priority. Define rules to control inbound and outbound traffic to your instance by configuring security groups. This ensures that your virtual server is protected from unauthorized access.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701794885906/923f8002-b83e-43d1-bb48-87e6db78d5e1.png align="left")
    
    In the Network settings, we are opting for the default VPC. In the security group configuration, we are allowing SSH (port 22), HTTPS (port 443), and HTTP (port 80) to ensure that the necessary network traffic can reach the instance securely.
    
7. **Add Storage:**
    
    Determine the storage capacity required for your instance. You can add and configure additional volumes depending on your application and data storage needs.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701795152307/a9513b67-1b82-4426-8dd0-bc8ab95c6592.png align="center")
    
    We are configuring 8 GiB of storage which is EBS storage.
    
8. **Review and Launch:**
    
    Before finalizing, review your configuration settings. If everything looks good, hit the "Launch" button. You'll be prompted to select or create a key pair for secure access to your instance.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701795542052/19226d73-df87-40d0-97b4-d24cd38f526a.png align="center")
    
    Congratulations! We've just initiated the launch sequence for our EC2 instance.
    

### Conclusion

To sum it up, AWS EC2 is like having your own virtual computer in the cloud. It's a smart way to use computing power without dealing with physical machines. We've learned how to set up an EC2 instance, and the cool thing is, it's not just about computers – it's about having a flexible and cost-effective solution for all sorts of projects.

So, with AWS EC2, you can focus on your work or projects without worrying about the technical details. It's like having a powerful, customizable computer on demand. As technology advances, EC2 keeps up, providing a reliable foundation for various applications. It's your ticket to a hassle-free and scalable computing environment in the cloud.