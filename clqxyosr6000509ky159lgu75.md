---
title: "Understanding AWS VPC: A Beginner's Guide"
datePublished: Wed Jan 03 2024 15:57:14 GMT+0000 (Coordinated Universal Time)
cuid: clqxyosr6000509ky159lgu75
slug: understanding-aws-vpc-a-beginners-guide
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1704297382877/334028e2-7315-49aa-805c-9f9e32fdb844.png
tags: aws, networking, vpc, vpc-basics

---

In the vast world of cloud computing, AWS (Amazon Web Services) stands out as a leading provider offering a wide array of services. One of the foundational services provided by AWS is the Virtual Private Cloud, commonly known as VPC. In this article, we'll explore the basics of AWS VPC in a simple and accessible manner, even for those who are not tech-savvy.

## What is AWS VPC?

Imagine AWS VPC as your own virtual network in the cloud. It allows you to launch AWS resources, such as virtual machines (EC2 instances), databases (RDS), and more, in a logically isolated section of the AWS Cloud. Essentially, it's like having your private piece of the internet within AWS.

## *What's AWS VPC in Plain English?*

*Imagine you have your own invisible room on the internet where you can put things – websites, apps, databases – and control who gets in or out. AWS VPC is just that – your virtual space to do things on the internet securely.*

## Key Concepts:

### 1\. **Subnets:**

*(Think of subnets like different shelves in your invisible room. Each shelf has its own purpose, and they're arranged in a way that if one gets messy, it doesn't mess up the others.)*

* Think of subnets as smaller segments within your VPC. Each subnet operates in a specific Availability Zone, providing high availability and fault tolerance.
    

### 2\. **Internet Gateway:**

*(The Internet Gateway is like a magical door. It lets your things inside your invisible room connect to the outside world – like a secret door to the internet.)*

* An Internet Gateway allows your VPC resources to connect to the internet. It's like the gateway to the outside world for your virtual network.
    

### 3\. **Security Groups:**

*(Security Groups are like security guards. They decide who or what can come into your invisible room and who or what can go out.)*

* Security Groups act as virtual firewalls for your instances. They control inbound and outbound traffic to and from your resources.
    

### 4\. **Route Tables:**

*(Route Tables are like the signs in your invisible room. They tell the things where to go – whether to stay inside (in your room) or go outside (to the internet).)*

* A Route Table defines the rules for routing network traffic. It determines where the traffic is directed, whether within the VPC or outside to the internet.
    

### 5\. **CIDR Blocks:**

*(CIDR Blocks are just a way to mark your territory. They help define the range of addresses your invisible room can use.)*

* CIDR (Classless Inter-Domain Routing) Blocks are a way to specify IP addresses and routing prefixes. They help define the range of IP addresses for your VPC.
    

## How Does it Benefit You?

### 1\. **Isolation:**

* VPC provides a secure and isolated environment for your resources, separating them from other users' resources on AWS.
    

### 2\. **Customization:**

* You have complete control over your VPC's IP address range, subnets, route tables, and gateways, allowing for a customized network architecture.
    

### 3\. **Security:**

* With Security Groups and Network Access Control Lists (NACLs), you can define and control the traffic to and from your instances.
    

## Setting Up Your First VPC:

1. **Sign in to AWS Console:**
    
    * If you don't have an AWS account, you can sign up for one. Once you're signed in, navigate to the AWS Management Console.
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704293741087/c16dd168-30ab-4b64-91f5-83d3119c656d.png align="center")
        
2. **Go to VPC Dashboard:**
    
    * In the AWS Management Console, find the "VPC" service. This is where you can create and manage your Virtual Private Cloud.
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704293782561/3ea54020-a7dc-4e87-8ee1-abf5dbb90b78.png align="center")
        
3. **Create a VPC:**
    
    * Click on "Create VPC" and follow the on-screen instructions. You'll need to specify the IP address range (CIDR block) for your VPC.
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704293869394/2a51ca9c-d0aa-4e0e-8a65-02c40dc1d8f6.png align="center")
        
    * Click on 'Create VPC'.
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704293927354/61daaee9-58b9-4c37-ac5c-997ab19021c3.png align="center")
        
        Your VPC has been created successfully.
        
4. **Configure Subnets:**
    
    * Create subnets within your VPC to organize your resources. Each subnet should be associated with a specific Availability Zone.
        
    * Select the 'Subnets' option under Virtual Private Cloud section located in the left panel.
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704294048618/26797726-bd95-43e8-96c1-0c4b14797357.png align="center")
        
        The above-showing subnets are the default ones which by default present with default VPC.
        
    * Let's create new subnets for our new VPC.
        
    * Click on 'Create Subnet'.
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704294169308/90a91531-ba54-4e4c-b40a-a58b54770c7c.png align="center")
        
        We want to add more subnet so scroll down and click on 'Add new subnet'.
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704294218617/a0f15c33-e84d-4e1c-afcb-727fad83392c.png align="center")
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704294259771/2bf471ad-84a6-471a-b87f-1e5888c6d930.png align="center")
        
        Click on 'Create subnet'.
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704294329828/5e0a7aeb-5d7e-4cc1-8623-9237a4a61b82.png align="center")
        
        Our both new subnets are ready now.
        
5. **Set Up Internet Connectivity:**
    
    * Create an Internet Gateway and associate it with your VPC to enable internet connectivity for your resources.
        
    * Select 'Internet gateways' option under Virtual Private Cloud section located in the left panel.
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704294396397/50512cbb-509c-49cf-8747-794f4c0f3dbf.png align="center")
        
    * Click on 'Create internet gateway'.
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704294446794/42b9b99f-79e2-418e-b077-e4b9d98f7d37.png align="center")
        
    * Your Internet gateway is created now and now it is asking to attach this to VPC.
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704294522714/2b22ce25-3ebd-40a0-a23d-182ceafcd521.png align="center")
        
    * Click on 'Attach to VPC'.
        
        Select your new VPC and click on 'Attach internet gateway'.
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704294598458/0b7a0790-ea5b-4543-bf95-004985757bc6.png align="center")
        
    * Now our New Internet gateway is attached to our created custom VPC.
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704294761035/e535673d-9a8d-4547-ac67-d270c56e2ca4.png align="center")
        
6. **Configure Route tables:**
    
    * After internet gateway setup, we need to configure our route table so that our machines get access to the internet. Without configuring this in route table, our machines will never reach to Internet.
        
    * Select the 'Route tables' option under Virtual Private Cloud section located in the left panel.
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704294908508/173c9a7e-1db3-4751-8de6-ce683c723e44.png align="center")
        
        The above two route tables are the default ones. The 1st route table shown above is of the default VPC and the 2nd one is created by default when we created custom VPC.
        
    * Let's create a new Route table for our custom VPC and not disturb the main route tables of the custom VPC.
        
    * Click on 'Create route table'.
        
        Give a name and select your VPC.
        
        ![]( align="center")
        
        Click on 'Create route table'.
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704295168044/eb1b87ea-63c3-4bd8-bc9d-18b14c13ffe5.png align="center")
        
        Now our custom route table has been created successfully.
        
    * Now we have to do two things, The first is 'Subnet associations' where we associate the subnets that we want to make public and connect to the internet.
        
        The second is 'Routes', where we need to add the route of our internet gateway to the internet.
        
    * Let's do this. First click on 'Subet associations' in the selected route table.
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704295399893/18e07cd3-7b06-4492-a630-01302675988b.png align="center")
        
        Click on 'Edit subnet associations'.
        
    * Select the required subnets and click on 'Save associations'.
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704295461879/e97a3d80-917e-4b34-8a99-5b4818eef1d6.png align="center")
        
        Now our subnet association has been succcessfully done.
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704295507500/d4e1c5d9-97a7-4d66-b332-800380c66995.png align="center")
        
    * Now click on 'Routes' option from the menu.
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704295547792/01c654ef-6088-4d56-a053-081e950ef31f.png align="center")
        
        Click on 'Edit routes'.
        
    * Click on 'Add route' and give the destination which is the internet in our case it is generally taken as 0.0.0.0/0 and select the target as Internet gateway, it will show your Internet gateway ID.
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704295654705/f89a6368-0af0-4d39-b832-1d42c3b38864.png align="center")
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704295666226/9527b8da-373b-4309-9dc6-9fe30f3305ea.png align="center")
        
        Click on 'Save changes'.
        
    * Our routes are now successfully configured.
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704295700786/947792e9-96ae-4354-bfcc-e55712c8dc17.png align="center")
        
7. **Launch Resources:**
    
    * Now that your VPC is set up, you can start launching resources like EC2 instances or databases within the defined subnets.
        
    * Let's launch an EC2 instance and see if we can ping google.com or not.
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704295794725/12d92bf9-7bd9-4285-a08e-83a12e083f05.png align="center")
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704295833483/3ace500d-233a-4793-a664-743fbb95d99e.png align="center")
        
        In the above, we have to click on 'Edit' in Network settings and select our custom VPC and custom subnets.
        
        Please note that 'Auto-assign public IP' is set to Disable by default.
        
        So enable it.
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704295953455/7d8199fb-e5d7-4721-9906-ec5c93cfa448.png align="center")
        
        I have allowed SSH and ICMP for the v4 address in the Security group. Leave everything as default and click on 'Launch instance'.
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704296059373/61fb71e4-dba4-4962-ae8c-afab653acc77.png align="center")
        
        We can see that this EC2 instance is launched under a custom VPC network. We can check this in the Details option by selecting that EC2 instance.
        
        Let's connect to this EC2 instance and ping google.com.
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704296244742/a3ffdfa3-f8c1-47fc-80e8-ce43e370df76.png align="center")
        
        Hurray! Our EC2 is successfully created in our custom VPC and network traffics are working fine for the custom VPC.
        

## Conclusion:

AWS VPC is the backbone of your cloud infrastructure, providing a secure, customizable, and scalable environment for your applications. While the technical details can get complex, especially for IT professionals, understanding the basic concepts empowers even non-tech individuals to appreciate the significance of VPC in the cloud computing landscape.

So, whether you're a small business owner or just curious about the world of cloud computing, AWS VPC is a fundamental concept worth exploring. It lays the groundwork for building robust and secure applications in the cloud.

Remember, the journey into the cloud begins with a single VPC, and AWS is here to guide you every step of the way. Happy exploring!