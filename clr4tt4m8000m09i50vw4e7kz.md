---
title: "A Beginner's Guide to AWS VPC NAT Gateway: Understanding and Hands-On Lab"
datePublished: Mon Jan 08 2024 11:15:01 GMT+0000 (Coordinated Universal Time)
cuid: clr4tt4m8000m09i50vw4e7kz
slug: a-beginners-guide-to-aws-vpc-nat-gateway-understanding-and-hands-on-lab
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1704706823939/560daffe-5d8b-4e30-bf9f-63ffe54f66c4.png
tags: aws, vpc, aws-certified-solutions-architect-associate, nat-gateway, aws-nat-gateway

---

Amazon Web Services (AWS) offers a powerful networking service called Virtual Private Cloud (VPC), allowing users to create a private, isolated section of the AWS Cloud. In this guide, we will explore one crucial aspect of AWS VPC – the Network Address Translation (NAT) Gateway. We'll delve into the basics, understand its purpose, and walk through a hands-on lab to set up and test a NAT Gateway in your AWS environment.

## Understanding NAT Gateway

### What is NAT?

Network Address Translation (NAT) is a technique used to map private IP addresses to a public IP address. In the context of AWS VPC, NAT allows instances in private subnets to initiate outbound traffic while masking their private IP addresses.

### Why Use NAT in AWS VPC?

Instances in private subnets lack direct internet access. NAT Gateway serves as a bridge, enabling these instances to communicate with the internet for tasks like downloading updates or accessing external resources while maintaining the security of the private subnet.

## Hands-On Lab: Setting Up AWS VPC NAT Gateway

### About lab

We are going to create a VPC with one Public Subnet and one Private Subnet. Instances in the Public subnet can access the internet, whereas instances in the Private subnet cannot.

We will create an Internet Gateway and attach it to our VPC.

Next, we will create a custom route table and associate only the public subnet. This ensures that only the public subnet can reach the internet, and the private subnet remains isolated.

To enable internet access for instances in the private subnet, we will create a NAT Gateway. The NAT Gateway will be placed in the public subnet as it requires internet access and an Elastic IP. After creating the NAT Gateway, we will add a route to it in our VPC's main route table, allowing instances in the private subnet to reach the internet through the NAT Gateway.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704703580648/75fe05b8-50b7-426c-adf8-7a0b4884eefa.png align="center")

### Prerequisites

1. **AWS Account:** Ensure you have an AWS account with the necessary permissions.
    
2. **VPC Configuration:** Set up a VPC with at least one private subnet.
    

### Step 1: Create VPC (all the required steps)

1. Create VPC.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704703697567/8882c92c-5ac3-4074-b7c2-d59f0570ab5f.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704703712183/39d8b39c-005f-4296-87ec-73e9375f5b4e.png align="center")
    
2. Create Subnets - Public & Private.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704703888014/b2c41768-0be4-44b0-9c7e-60dce76b1af2.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704703921353/5fa555b1-f083-47d8-b1bd-89b9303a4415.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704703947932/1ea5b555-2c68-4273-a687-af1a4fdbf45f.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704703968472/d728bffb-f190-4c05-826f-0d41db548277.png align="center")
    
3. Create an Internet Gateway and Attach it to the VPC.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704703988862/eea49438-97da-4c3c-9723-626178e929a6.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704704000508/7bd42c60-37c9-40a3-bfa9-03b22fd988eb.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704704011741/fb57ee0e-43d9-46e3-bb5d-5d86b0f2adbb.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704704021736/50cce7ae-0322-47fc-9dce-247c447e29ec.png align="center")
    
4. Create a Route table and modify it.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704704038566/6ce4f841-330d-4969-a423-1648c8995502.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704704059230/18dd29d9-63e5-4a15-80df-622f73fe5304.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704704086335/4c36d366-910a-4288-8a65-51488192e489.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704704101834/64acab18-bfd0-47f4-b815-cb5edaea9f27.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704704123738/2ff75c97-7337-4c01-a1be-88f4b603859a.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704704154313/e55c1805-f543-4a05-b638-fd3b0697474f.png align="center")
    

### Step 2: Create EC2 Instances - one in Public and one in Private

**Public Server - (Must have public IP)**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704704823543/ab629b8f-8461-4fb5-a14a-ade77b08f37a.png align="center")

**Private Server - (Only have private IP)**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704704859946/db9f1793-0cbf-4abe-ade5-dd91d9d512fe.png align="center")

### Step 3: Check Connectivity

Take console of Public server and try to ping the private server IP.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704705010495/b9e10db3-66cf-49f4-85d0-5de316a986c6.png align="center")

Let's try to take the console of private server.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704705039052/e3461f8c-fadc-456c-9ed1-844bcb8433b3.png align="center")

We can't take a remote console as this server does not have a Public IPv4 address.

Let's login to this private server by its private IP via the public server.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704705174988/ed3c3601-2762-4155-9fb8-4ef69e6c9bcf.png align="center")

We successfully logged into the private server via the public server, technically known as a Jump server.

Now, attempt to ping [google.com](http://google.com) from the private server. It should not be reachable since this instance is created under a private subnet.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704705269367/da58a12b-9d27-410a-a44b-401421eb9982.png align="center")

It is not able to reach the google.com or Internet.

So to resolve this issue, we have to create a NAT Gateway.

### Step 4: Create NAT Gateway

1. **Navigate to the VPC Console:**
    
    * Log in to the AWS Management Console.
        
    * Go to the VPC Dashboard.
        
2. **Create NAT Gateway:**
    
    * Select "NAT Gateways" from the left pane.
        
    * Click "Create NAT Gateway."
        
    * Choose your VPC and a subnet for the NAT Gateway.
        
    * Allocate an Elastic IP address.
        
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704705757535/c04163f7-02eb-45bf-ba59-71db570165e5.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704705784721/b8c2d047-9565-4891-9745-e6f56f020eaf.png align="center")
    
3. **Review and Create:**
    
    * Review your settings.
        
    * Click "Create NAT Gateway."
        
    * Wait for the NAT Gateway to be created.
        
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704705813035/e17a3436-8f8f-45cd-9746-4ed1139c4a15.png align="center")
    

### Step 5: Update Route Tables

1. **Navigate to Route Tables:**
    
    * In the VPC Dashboard, select "Route Tables."
        
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704705858974/afb4c077-d158-4de8-8211-31bfe2c64709.png align="center")
    
2. **Update Route Table:**
    
    * Edit the route table associated with your private subnet.
        
    * Add a route with the destination `0.0.0.0/0` pointing to the NAT Gateway.
        
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704705954833/1d0c8bc5-13ed-4d7a-8f31-35454e46102a.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704705978069/3b7e71f2-d896-43f6-aab4-225782533d13.png align="center")
    

### Step 6: Test the Configuration

1. **Internet Access Test:**
    
    * Connect to the private instance via public instance.
        
    * Ping [google.com](http://google.com) to check if the private server can now reach the internet.
        
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704706212048/04397196-222a-4c5b-a857-aa4a2e7d3749.png align="center")
    
    Hurray! we can reach to the internet via Private server with the help of NAT gateway.
    

## Conclusion

Congratulations! You've successfully set up and tested an AWS VPC NAT Gateway. This essential networking component plays a crucial role in enabling outbound internet connectivity from private subnets, ensuring secure and controlled communication.

As you continue your AWS journey, consider exploring other VPC features and scenarios. Experiment with different configurations to gain a deeper understanding of AWS networking. Hands-on experience is key to mastering these cloud services.

Feel free to reach out to the AWS documentation and AWS community for further exploration. Happy learning, and may your cloud adventures be both educational and enjoyable!