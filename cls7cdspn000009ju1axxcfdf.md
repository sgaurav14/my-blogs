---
title: "Understanding NAT Instances in AWS: A Beginner's Guid"
datePublished: Sun Feb 04 2024 10:10:13 GMT+0000 (Coordinated Universal Time)
cuid: cls7cdspn000009ju1axxcfdf
slug: understanding-nat-instances-in-aws-a-beginners-guid
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1707041177970/6cf68530-a6fd-440a-bbee-e3076d499078.png
tags: aws, vpc, nat-instance, aws-networking

---

If you're venturing into the world of AWS (Amazon Web Services), you've likely come across the term NAT (Network Address Translation). NAT plays a crucial role in enabling instances in a private subnet to communicate with the internet while keeping them secure. In this guide, we'll demystify NAT instances in AWS and provide practical examples to help you grasp the concept.

## **What is NAT?**

Network Address Translation (NAT) is a method used to map private IP addresses to a public IP address. This process is essential for instances in a private subnet to access the internet while maintaining a level of security. NAT instances act as intermediaries, forwarding traffic from private instances to the internet and vice versa.

## **Why Use NAT Instances?**

In AWS, private instances typically reside in private subnets, isolated from direct internet access. NAT instances serve as a bridge between the private instances and the internet, allowing them to communicate without exposing their private IP addresses.

## **Setting Up a NAT Instance: Step by Step**

### **Lab Objectives:**

In this lab, we will perform the following tasks:

1. Create a custom VPC.
    
2. Establish two subnets - Public and Private.
    
3. Set up an Internet Gateway and attach it to the custom VPC.
    
4. Create a Route Table and configure a route for internet access. Associate the Public Subnet with this route table to enable internet access for machines within this subnet.
    
5. Launch an EC2 instance in the Public Subnet with a Public IP address.
    
6. Launch an EC2 instance in the Private Subnet without a Public IP.
    
7. Attempt to ping [www.google.com](http://www.google.com) from both EC2 instances.
    
    * The public EC2 instance should successfully reach [www.google.com](http://www.google.com).
        
    * The private subnet instance should not have access to [www.google.com](http://www.google.com), adhering to protocol.
        
8. If the above steps are correct, proceed to create a NAT instance in the Public Subnet.
    
9. Disable the source/destination check on the NAT instance.
    
10. Add a route to the internet in the main route table of the VPC, directing traffic through the NAT instance.
    
11. Recheck the reachability of [www.google.com](http://www.google.com) from both servers.
    

* This time, the private EC2 instance should successfully reach [www.google.com](http://www.google.com), facilitated by the NAT instance.
    

Following these steps will help you understand and implement a secure network architecture within AWS, utilizing public and private subnets with the necessary configurations for internet access and communication.

### **Step 1: VPC Status**

**VPC -**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706987127466/cd98c32c-c545-474c-9617-ec7f7e9be8e1.png align="center")

**Subnet -**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706987210225/82acc8de-898e-483a-a85b-67b7fcf6c31d.png align="center")

**Internet Gateway -**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706987253660/51729860-d449-4b07-9d51-00068698bc4e.png align="center")

**Route table -**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706987349245/599c73ca-4c5a-4add-9ae8-5375afd88217.png align="center")

**Routes -**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706987379635/aaec1858-5907-4775-9401-fa2def1a0e84.png align="center")

**Subnet-Associations -**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706987402554/b817e623-9833-4725-9660-676149777624.png align="center")

### **Step 2: EC2 Status**

**Public Server - (having public IP)**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1707038875335/1db9e39c-8df2-46ba-965a-934ce4328152.png align="center")

This machine can reach internet.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1707039086823/3f022b08-5c8b-4002-b4f1-dbc540a680f2.png align="center")

**Private Server -**

This machine have not Public IP address, so we can't connect this machine directly. To connect this private server, we need a Jump server which is an intermediary gateway in a network, enhancing security by providing controlled access to internal servers from external networks.

So we will use our Public server as a Jump server and we will ssh our private server from there.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1707039368366/67b55717-14f9-4916-ac7e-7359961a01b0.png align="center")

Let's try to ping www.google.com from private-server.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1707039419224/52382603-2c6e-432f-bdd9-f9add271231c.png align="center")

It is not able to reach the google.com. If we want to reach the google.com then we need an NAT gateway or NAT instance. We already covered NAT instances in our last blog. This time we will use a NAT instance.

### **Step 3: Launching a NAT Instance**

Let's deploy a NAT instance from the AWS EC2 dashboard.

We only need to select the AMI in section "browse more AMIs" and we need to search for NAT instance.

Also make sure that this instance will also create in our custom VPC and public subnet and must have public IPv4 address.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1707040112325/bb99c2e6-4c3b-4a78-9c1b-b106f345ee04.png align="center")

### **Step 4: Disabling Source/Destination Check**

1. **EC2 Dashboard:** Navigate back to the EC2 Dashboard.
    
2. **Select NAT Instance:** Select your NAT instance.
    
3. **Actions &gt; Networking &gt; Change Source/Dest. Check:** Disable the source/destination check.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1707040150775/ad361c44-da9d-4431-aa63-1f930d8de014.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1707040169089/7f0be770-8622-4fa1-bb3c-75571449dc78.png align="center")
    

### **Step 5: Configuring Route Tables**

1. **Navigate to VPC Dashboard:** Go to the VPC Dashboard in the AWS Management Console.
    
2. **Route Tables:** Click on "Route Tables" in the left-hand menu.
    
3. **Edit Route Table:** Edit the main route table associated with your custom VPC.
    
4. **Add Route:** Add a route with destination `0.0.0.0/0` and target as the NAT instance ID.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1707040300712/9565bb9b-29fa-4481-b1ad-185875deb4c9.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1707040327729/841051fa-c376-4974-8d26-2c6211330a61.png align="center")
    
    Now we have added the route. Its time to again check the connectivity.
    

### **Step 6: Testing Internet Connectivity**

1. Let's again ssh to the private server from public server.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1707040455609/442f4d7c-f666-437c-a59a-b17921da0dc3.png align="center")
    
2. Now let's ping www.google.com from our private server.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1707040507434/76eba13a-d9a9-4ff5-9100-c859f572e957.png align="center")
    
    Hurray!! with the help of NAT instance, our private subnet servers can reach to internet. By using this, we will make our environment make secure.
    

Congratulations! You've successfully set up a NAT instance in AWS, allowing instances in your private subnet to access the internet securely.

## **Conclusion**

Understanding NAT instances in AWS is crucial for creating secure and well-connected VPC architectures. This guide aimed to provide beginners with a practical overview of NAT instances, from launching an instance to configuring route tables. As you continue your AWS journey, mastering networking concepts like NAT will undoubtedly enhance your ability to design and manage robust cloud environments. Happy cloud computing!