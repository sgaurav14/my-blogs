---
title: "AWS Basics: Build Highly Available and Scalable Apps with ELB and ASG"
datePublished: Fri Dec 06 2024 16:01:55 GMT+0000 (Coordinated Universal Time)
cuid: cm4cxoqpy000009i7247j2cfm
slug: aws-basics-build-highly-available-and-scalable-apps-with-elb-and-asg
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1733500833008/2d65122a-a5f0-4176-94fe-fb5146af19f1.png
tags: aws, load-balancer, load-balancing, aws-certified-solutions-architect-associate, network-load-balancer, auto-scaling-aws, high-availability, classic-load-balancer, applicationloadbalancer

---

#### **Introduction**

When building applications in the cloud, it’s important to make them available all the time (high availability) and ready to handle more users when demand increases (scalability). AWS makes this easy with tools like Elastic Load Balancing (ELB) and Auto Scaling Groups (ASG). In this guide, we’ll break these concepts down and show you how they work in real-life scenarios.

---

### **What Are High Availability and Scalability?**

1. **High Availability (HA):**  
    Imagine your favorite shopping site always being online, even during big sales or outages. AWS achieves this by running your app in multiple locations, called *Availability Zones (AZs)*. If one zone fails, the others keep your app running.
    
2. **Scalability:**
    
    * **Vertical Scaling:** Think of upgrading your car from a sedan to a truck to carry more load. In AWS, this means upgrading your server (EC2 instance).
        
    * **Horizontal Scaling:** Like adding more cars to carry more people, AWS can add more servers when traffic increases and remove them when traffic slows.
        

---

### **Elastic Load Balancing (ELB): Distributing the Load**

ELB is like a traffic cop that directs user requests to the best available servers. This ensures your app stays responsive and avoids overloads.

#### Types of Load Balancers:

1. **Classic Load Balancer (CLB):**  
    The simplest option, useful for older applications. It can handle basic web traffic (HTTP/HTTPS) and database requests (TCP).
    
2. **Application Load Balancer (ALB):**  
    Great for modern apps. It understands web traffic and can direct users to specific parts of your app (e.g., showing a shopping page for `/shop` and a login page for `/login`).
    
3. **Network Load Balancer (NLB):**  
    Best for high-speed, low-latency traffic like gaming servers or video streaming. It can handle millions of requests per second.
    
4. **Gateway Load Balancer (GWLB):**  
    Ideal for advanced scenarios like connecting to third-party firewalls or security systems.
    

---

### **Key Features of ELB**

1. **Sticky Sessions:**  
    Imagine a user browsing your app. With sticky sessions, they’ll always be directed to the same server to keep their session intact. This is handy for apps like shopping carts.
    
2. **Cross-Zone Load Balancing:**  
    ELB ensures that user requests are distributed evenly across all servers in different AZs. This prevents overloading any single zone.
    
3. **SSL Certificates:**  
    These encrypt user data, such as passwords or payment details. AWS makes it easy to add SSL using AWS Certificate Manager (ACM).
    
4. **Connection Draining:**  
    When removing a server for maintenance, ELB lets existing user requests finish before shutting it down to avoid interruptions.
    

---

### **Auto Scaling Groups (ASG): Scaling Up and Down Automatically**

An ASG automatically adjusts the number of servers your app uses based on traffic.

#### How ASG Works:

1. **Launch Configuration or Template:**  
    Think of it as a blueprint for creating servers. It specifies the server type, storage, and settings.
    
2. **Scaling Policies:**  
    These decide when to add or remove servers:
    
    * **Target Tracking Scaling:** Keeps a specific metric (like CPU usage) steady. For example, add servers if CPU goes over 70%.
        
    * **Step Scaling:** Adds or removes servers in steps based on traffic spikes.
        
    * **Scheduled Scaling:** Adds or removes servers at specific times, like adding more servers during office hours.
        
3. **Health Checks:**  
    ASG checks if a server is working. If not, it replaces it with a new one.
    

---

### **Lab: Building a Scalable and Highly Available App**

Here’s how to set up an AWS lab to understand ELB and ASG.

#### **Step 1: Create an EC2 Instance**

1. Log in to the AWS Management Console.
    
2. Launch an EC2 instance using a Linux AMI.
    
3. Install a simple web server, like Apache:
    
    ```bash
    sudo yum install httpd -y
    sudo systemctl start httpd
    echo "Welcome to Server 1" > /var/www/html/index.html
    ```
    

#### **Step 2: Set Up an Auto Scaling Group**

1. Go to the EC2 dashboard and create a Launch Template.
    
2. Define the instance type and security group (allow HTTP traffic).
    
3. Create an Auto Scaling Group using the template and specify at least two AZs.
    

#### **Step 3: Add an Elastic Load Balancer**

1. Go to the ELB dashboard and create an ALB.
    
2. Add the Auto Scaling Group as the target for the load balancer.
    
3. Enable cross-zone load balancing and HTTPS (using an SSL certificate from ACM).
    

#### **Step 4: Test the Setup**

1. Access the load balancer’s DNS name in your browser.
    
2. Scale up by increasing traffic using a load-testing tool or adding users.
    
3. Watch the Auto Scaling Group add more instances and the ELB distribute traffic evenly.
    

---

### **Conclusion**

AWS makes it simple to build apps that are always available and ready for growth. With Elastic Load Balancing and Auto Scaling, you can ensure your app performs well, even during high traffic. Try setting up the lab to see these concepts in action and start building scalable apps today!