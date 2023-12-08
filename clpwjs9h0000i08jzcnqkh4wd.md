---
title: "Scaling Safely: A Simple Guide to Autoscaling, Load Balancers, and Web Security"
datePublished: Fri Dec 08 2023 11:32:33 GMT+0000 (Coordinated Universal Time)
cuid: clpwjs9h0000i08jzcnqkh4wd
slug: scaling-safely-a-simple-guide-to-autoscaling-load-balancers-and-web-security
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1702035674487/9c55c22a-f26b-44bb-ada2-e3ca64831b23.png
tags: aws, load-balancing, auto-scaling-aws, applicationloadbalancer, webapplicationfirewall

---

### **Introduction:**

In today's digital age, ensuring a smooth online experience is crucial for businesses. In this blog post, we'll explore how Autoscaling Groups, Application Load Balancers (ALB), and Web Application Firewalls (WAF) work together to keep websites running smoothly, even during traffic spikes.

### **Section 1: Autoscaling Groups - Your Website's Safety Net**

Imagine your website is like a virtual storefront. Sometimes, there's a surge in customers, and you want to ensure your "store" doesn't get overcrowded and crash. That's where Autoscaling Groups come in.

**What is an Autoscaling Group?**

An Autoscaling Group is like having an army of servers ready to help during busy times and gracefully step back during quieter moments. It dynamically adjusts the number of servers based on the current demand, ensuring your website remains responsive and available.

### **Section 2: Application Load Balancers - Traffic Directors**

Now, think of an Application Load Balancer as a smart traffic director for your website.

**What is an Application Load Balancer?**

Just like a traffic cop directs cars on the road, an Application Load Balancer distributes incoming web traffic across multiple servers. This not only prevents any one server from getting overwhelmed but also ensures a faster and more reliable experience for your visitors.

### **Section 3: Web Application Firewall - Guardian at the Gate**

Security is paramount, and that's where the Web Application Firewall steps in.

**What is a Web Application Firewall?**

Picture the Web Application Firewall as a shield that protects your website from potential threats. It analyzes incoming traffic and filters out malicious requests, safeguarding your website and its visitors from cyber attacks.

### **Section 4: The Power of Integration**

Now, let's see how these three components work seamlessly together.

**The Dynamic Trio: Autoscaling, Load Balancing, and WAF**

1. **Smooth Scaling:** As traffic increases, Autoscaling Groups add more servers, while the Load Balancer distributes the load efficiently.
    
2. **Security First:** The Web Application Firewall keeps a watchful eye, ensuring only legitimate traffic reaches your servers, protecting against potential threats.
    
3. **Continuous Balance:** Even during fluctuations in traffic, this dynamic trio maintains a perfect balance, ensuring a reliable and secure user experience.
    

## **Section 5: Bringing It All Together - A Real-world Example**

Now that we understand the basics, let's dive into a real-world project that demonstrates the power of Autoscaling Groups, Application Load Balancers, and Web Application Firewalls working together harmoniously.

**Project Overview: Creating a Scalable and Secure Web Environment**

In this project, we'll walk through the steps of setting up a scalable and secure web application using these three essential components.

### **Step 1: Setting up Autoscaling Groups**

1. Navigate to the AWS dashboard, go to the EC2 section, and at the bottom, select "Auto Scaling Groups.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702022649299/a4829537-0942-4e84-b4e0-e0436e4d8c1a.png align="center")
    
2. Upon selecting "Auto Scaling Groups," you will be directed to the Amazon EC2 Auto Scaling page.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702022695527/095c415b-2742-4c2a-b692-deb92b4ec2cc.png align="center")
    
    Select "Create Auto Scaling Group" from the options presented on the Amazon EC2 Auto Scaling page.
    
3. After clicking it, you will be presented with a template to fill out, including the Autoscaling group name. Additionally, select your Launch template, which we previously discussed in our blog [https://sgaurav.hashnode.dev/simplified-aws-ec2-templates-launch-modify-and-deploy-with-ease](https://sgaurav.hashnode.dev/simplified-aws-ec2-templates-launch-modify-and-deploy-with-ease).
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702022816499/92e061b7-409f-4630-b625-74e79d8c9bcb.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702022844531/96e6d95b-2170-481f-b34a-015feabb7593.png align="center")
    
    Following these steps, click on "Next" to proceed with the configuration.
    
4. In the "Choose instance launch options" section, select your network, including the VPC and subnets.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702022937919/69fc9c76-95f5-444d-a749-f095f263876a.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702022965906/e200eb2d-9d8c-44ae-9f65-184f669b01b7.png align="center")
    
    Once you have selected your network options, proceed by clicking on "Next."
    

### **Step 2: Configuring the Application Load Balance**

1. In the "Configure advanced options" section, configure your load balancer. In this scenario, select "Application Load Balancer" and provide a name for the load balancer.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702025097204/40e9d444-0dc5-4a50-8409-38c2c36fdcaf.png align="center")
    
2. Review and verify all your VPC and subnet settings to ensure they are accurate and aligned with your configuration.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702025167640/4406abb6-4a17-4e94-9475-81c2b617b87b.png align="center")
    
3. Adjust the "Listeners and routing" settings by selecting port 80 since our application operates on that port. For routing the traffic from the internet to specific target groups, create a new target group by clicking on the option "Create a target group."
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702023344099/eba9b889-d85f-4985-923a-9f377be2b184.png align="center")

Skip the VPC Lattice Integration options if they are not applicable to your current configuration or project requirements.

1. Configure a health check for your application load balancer to ensure the health and responsiveness of your application.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702023422428/b56d7e05-8a9d-458a-8203-c40b05c13b92.png align="center")

Here, choose "Turn on Elastic Load Balancing health checks" to enable regular checks on your EC2 instances. If any issues are detected, the system will automatically launch another instance. Additionally, set the Health Check grace period to 300 seconds, allowing the load balancer sufficient time to assess the health of its resources (EC2 instances).

1. If desired, enable CloudWatch to monitor and collect metrics for your setup. This step allows you to keep track of various performance metrics and gain insights into the behavior of your resources.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702024021222/4180fed1-d735-4ab7-98c4-e88bc2d2bcab.png align="center")

After configuring CloudWatch settings, proceed by clicking on "Next."

1. In this crucial section, configure your Load Balancer to initiate the Desired Capacity (for normal operation), set the Min Desired Capacity (ensuring at least this number of EC2 instances are up and running), and specify the Max Desired Capacity (allowing for the launch of instances up to this maximum when there's an increase in load).
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702024174153/299bdb5d-7f0a-4835-a6af-ac5bf5a5b3e4.png align="center")
    
2. Configure the scaling policy to determine when to monitor, distribute, launch, and take necessary actions based on the specified conditions. This policy ensures dynamic adjustments to your resources in response to changing demands.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702024260074/72712d7c-4491-4d96-ac10-88b924084669.png align="right")

In this step, opt for the Target Tracking scaling policy. Specify the policy to monitor the Average CPU utilization, and when it exceeds 20% (target value), initiate the launch of the necessary number of instances. The Instance Warmup option determines the time it takes to bring the new EC2 servers into operation.

1. Choose the Instance Maintenance Policy based on your requirements. This policy defines how instances are replaced during routine maintenance, providing flexibility to balance cost and availability.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702024404463/7ec3d8d3-903c-4a42-a68c-e5f3a7a8d135.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702024446084/f23db771-ebde-4b4d-877d-4b6ed1426624.png align="center")

Skip the remaining options and proceed by clicking on "Next." This will take you to the final steps for reviewing and confirming your configurations before completing the setup.

1. Configure a notification service for your load balancer and autoscaling group if needed. This service will keep you informed about important events and changes in your environment.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702024504688/443ca5ca-16da-4aa5-9ef8-a115cebdaac8.png align="center")
    
2. Assign tags if deemed necessary. Tags are useful for organizing and categorizing resources, providing an additional layer of organization and identification.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702024543068/16ea45e5-cc5d-42ec-b25b-e8b12b7a4e97.png align="center")

Great! By assigning the name tag with the value "my-aws-server," each new instance launched as per the load will be identified with the name "my-aws-server." This makes it easier to manage and distinguish instances in your environment.

1. Take a moment to review all your configurations, ensuring that each setting aligns with your intended setup. Verify that the specified parameters for the autoscaling group, load balancer, notifications, and tags are accurate and meet your requirements.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702024625691/724dc7a8-5e01-4fb1-8ffa-a7306589071d.png align="center")

Once you've thoroughly reviewed all the configurations and ensured they align with your requirements, proceed by clicking on "Create Auto Scaling group." This will initiate the creation process based on your defined settings.

1. Congratulations! Your autoscaling group is now successfully created. You can verify this by checking the dashboard, where you should see relevant information and status indicators confirming the creation of the autoscaling group.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702024691508/2226d684-3e2a-45f9-a5c1-0c737cba38f6.png align="center")
    
2. Fantastic! Your configuration is working seamlessly. In just seconds of creating the autoscaling group, an instance named "my-aws-server" was launched, aligning with the desired instance setting configured earlier for the load balancer. This dynamic scaling demonstrates the flexibility and responsiveness of your autoscaling setup.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702024780784/de800c8b-ba5a-41fe-8f37-fbda4599e21f.png align="center")

Excellent news! With the successful launch of the "my-aws-server" instance, your application is now up and running, demonstrating the effectiveness of your autoscaling group and load balancer configuration. This setup ensures that your application remains responsive and available, adapting to changing demands.

### **Step 3: Implementing Web Application Firewall**

1. Navigate to the AWS dashboard and use the search function to find "WAF." From the search results, select "WAF and Shield."
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702025493842/6f7f0b78-0aae-40c0-9f92-55b5d628b0dd.png align="center")
    
2. Upon selecting "WAF and Shield," you will be directed to the Web Application Firewall (WAF) dashboard.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702025536246/4d3ad5fa-004f-4773-9c84-0491bada2758.png align="center")
    
    Click on "Create Web ACL" to initiate the process of creating a Web Application Firewall Access Control List (Web ACL).
    
3. Upon clicking, you'll be presented with a template form. Fill in the required details, including selecting the region and providing a name for your Web ACL.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702025625512/abd99fa7-491c-4206-a336-d667bf50b03e.png align="center")
    
4. Choose the AWS resource to which you want to configure security by specifying the appropriate details in the Web ACL configuration. This could include selecting the resource type, such as an Application Load Balancer or CloudFront distribution.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702025674116/33ec5956-cf7f-4302-9df5-ed2cb59ee70a.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702025686362/3fafc55b-a85e-4caa-bc86-8e18365fe4b4.png align="center")
    
    Choose "Application Load Balancer" as the resource type, and it should automatically display your application load balancer name. Select the appropriate load balancer and click on "Add" to include it in the Web ACL configuration.
    
5. Great! With the successful selection of your Application Load Balancer as the resource, your configuration is on track. This resource is now integrated into the Web ACL for security configuration.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702025750607/69b4f33a-53a8-4936-a848-e945244b761c.png align="center")
    
    Proceed to the next step by clicking on "Next." This will take you through the additional configurations and settings to tailor the Web ACL according to your security requirements.
    
6. Select rules for your Web ACL to define the security policies and actions that should be enforced. This step allows you to specify the criteria and conditions under which traffic is allowed or blocked.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702025872214/976e4a8b-f912-40fd-9aae-25175d5bc1e6.png align="center")
    
    Click on "Add rules" and choose "Add managed rule groups" to include predefined rule sets that can help protect your web application against common security threats and vulnerabilities.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702025909579/c8f1fb97-5510-459c-8a78-9efe904baecf.png align="center")
    
7. Upon selecting "Add managed rule groups," you will be presented with options for managed rule groups.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702025942023/d478a828-4bbe-42ef-b3ae-5d7cc4b77b83.png align="center")
    
    Under the AWS managed rule groups, search for options related to bot protection. Look for rules specifically designed to enhance security against bot-related threats. This could include rules targeting bot behavior, scraping, or other malicious activities. Select the appropriate bot-related rules to bolster your application's security against automated threats.
    
8. Upon locating the bot-related rules, select the option and choose "Edit." This will allow you to customize and fine-tune the settings for bot protection according to your specific requirements and the nature of your application traffic.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702026030409/b1ea4ec1-4a7b-4ab0-a5ed-4846dfbc32de.png align="center")
    
9. After selecting "Edit," you'll be directed to another screen for "Add managed rule groups." This is where you can further refine and customize the settings for the selected managed rule groups, particularly the ones related to bot protection.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702026101368/f64b437c-1b32-472d-a947-0d9f1048caec.png align="center")
    
    For the Bot Control Inspection level configuration, choose the common option that aligns with your security needs. This level determines the depth of inspection applied to incoming traffic to identify and mitigate potential bot-related threats. Select an appropriate inspection level based on your application's requirements and sensitivity to automated traffic.
    
10. Select the scope for Inspection and Bot control rules. This involves defining the extent to which the inspection and bot control rules should be applied. Consider the specific requirements and characteristics of your application traffic when configuring the scope.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702026214985/fffe7a75-b64d-407d-878d-a18a3bae10be.png align="center")
    
    Specify the scope to "Inspect all web requests" to comprehensively analyze all incoming traffic. As for the Bot Control rules, you can leave them as default if they align with your security objectives. This configuration ensures a thorough inspection of web requests while applying default bot control rules to enhance security.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702026286860/447fe1e6-60fa-47aa-9b2d-61d2629288ac.png align="center")
    
    Once you have configured the Inspection level and Bot Control rules scope, scroll down and click on "Save rule" to apply the changes. This step finalizes the customization of the bot-related rules within your Web ACL.
    
11. Great job! By clicking "Save rule," you have successfully saved the customized rules for bot protection within your Web ACL. This enhances the security posture of your application against automated threats and ensures a more robust defense mechanism.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702026335759/2d806462-bde2-4bde-9acd-625cf40c94e8.png align="center")
    
12. In the "Set rule priority" section, click on your rule, and then proceed to the next step by clicking on "Next." This allows you to prioritize and organize the rules within your Web ACL to ensure effective enforcement of security policies.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702026381441/9ac3bace-5fcd-4a99-a91d-35f8c148e4d3.png align="center")
    
13. In the "Configure metrics" section, choose the metrics you want to configure based on your monitoring needs. After making your selections, proceed by clicking on "Next" to move forward in the configuration process.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702026439690/eed9c967-25b9-497e-86fb-ab1e43e73c1e.png align="center")
    
14. Take a moment to review your Web ACL configuration, ensuring that all settings, rules, and priorities align with your security requirements. This review step is crucial to confirming that your Web ACL is properly configured to protect your application against various threats.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702026492249/546faa29-101a-4f68-b5a4-01c63f0faace.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702026528022/e7746b1c-5729-491d-8635-95d1fe3ef3ea.png align="center")
    
    After reviewing your Web ACL configuration, click on "Create web ACL" to finalize and deploy the Web Application Firewall settings for your specified resource. This action completes the setup of your Web ACL with the configured security rules.
    

Congratulations! With the successful creation and configuration of your Web ACL, your application is now equipped with an additional layer of security, particularly against bot-related threats. This proactive measure enhances the overall resilience of your web application and contributes to a more secure online environment.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702026568767/6b0dd68d-1dd5-409f-a3bd-dcf7696f364f.png align="center")

### **Step 4: Testing and Observing**

1. Log in to your application EC2 instance which was by default launched after creating an autoscaling group.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702026671980/1e11b929-8e3a-461d-80d0-893fc52cb086.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702026683328/d5349ddf-7075-4e51-9aaa-0735d1b1ab52.png align="center")
    
2. We are conducting testing, and as it may not be feasible to generate organic traffic on our server, we will simulate increased CPU load manually. According to our configured rule, if the CPU utilization exceeds 20%, the autoscaling group will initiate the launch of additional EC2 instances for the same application, up to a maximum of 5 servers (Max Desired Capacity).
    
    To simulate CPU load, we will use the 'stress' command. Let's begin by installing this command and imposing a 50% CPU load on the server
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702026863232/ac492ac2-4ec6-4894-9e80-b64dbef4d013.png align="center")
    
    We have successfully installed the 'stress' package. Now, let's intentionally increase the CPU load on our server for testing purposes.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702026884768/cc77053f-9c42-42cf-b992-58cf0d230259.png align="center")
    
3. After some time, as the load increases, we can observe on the EC2 dashboard that new instances are automatically launched in response to the heightened demand.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702026951819/6129e035-9815-430c-803f-2cbef9bb179d.png align="center")
    
    And within a few seconds, all the instances are up and running, effectively scaling to accommodate the increased load.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702026991817/dea2a90d-c05f-423c-a523-18f40ec0419d.png align="center")
    

So, it's confirmed that our Autoscaling and load balancer are functioning as expected. Now, let's simulate a decrease in load by terminating the stress command and observe the system's response.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702027074243/840a4db0-d3d9-4f9f-b750-5ba40bc283e2.png align="center")

And as the load decreased, the EC2 instances automatically began to terminate, demonstrating the dynamic nature of the autoscaling group in response to varying workloads.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702027116579/c7c15b9f-e92f-4030-804c-c9dc2f6e778c.png align="center")

After a few minutes, as there was no load on the initial server, all the other four EC2 instances were automatically terminated. This successful outcome indicates that our lab, including the autoscaling group and load balancer, has been configured and tested effectively.😊😊.

### Cleaning up your environment:

As our lab scenario is successful, let's delete all the resources we created.

1. First delete the load balancer.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702027467472/d41d313a-4485-4351-ae0d-f09e4c69aab9.png align="center")
    
    Select your load balancer, click on "Actions," and then choose "Delete Load Balancer."
    
2. Now, let's delete our target groups as well.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702027538943/2930ae1c-2043-4dfd-ab0b-77921c3ece1b.png align="center")
    
3. And finally, delete the autoscaling group.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702027575082/151f2229-2aaf-4de9-a5d3-2c8fa7b1d375.png align="center")
    
    Select you autoscaling group, click on actions and then click on Delete.
    
4. Once we delete our Autoscaling group, the initial instance that was automatically initiated will now terminate itself, as the Autoscaling group responsible for its management has been removed.
    

### **Conclusion:**

In this project, we've witnessed how Autoscaling Groups, Application Load Balancers, and Web Application Firewalls work together to create a resilient, scalable, and secure web environment. As we embrace the dynamic nature of web traffic, these technologies play a pivotal role in ensuring a seamless and protected user experience.

By understanding the importance of these components and witnessing their collaboration in action, we're better equipped to build and maintain web applications that can scale with the demands of the digital landscape while prioritizing the safety of both data and users.