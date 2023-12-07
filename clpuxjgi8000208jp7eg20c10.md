---
title: "Simplified AWS EC2 Templates: Launch, Modify, and Deploy with Ease"
datePublished: Thu Dec 07 2023 08:22:04 GMT+0000 (Coordinated Universal Time)
cuid: clpuxjgi8000208jp7eg20c10
slug: simplified-aws-ec2-templates-launch-modify-and-deploy-with-ease
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1701937226476/1e3e34dd-a687-4831-b5a9-a010816728be.png
tags: aws, aws-ec2, 90daysofdevops, awstemplate, awswithtws-7daysofaws, userdatascriptsaws

---

We learned how to deploy an AWS EC2 instance in the blog [https://sgaurav.hashnode.dev/renting-servers-in-the-cloud-a-laymans-introduction-to-aws-ec2](https://sgaurav.hashnode.dev/renting-servers-in-the-cloud-a-laymans-introduction-to-aws-ec2).

Typically, after launching an EC2 instance, we would log in to the server and set up our application or services manually for example nginx. However, when dealing with a scenario where we need to deploy Nginx on hundreds of servers, a more efficient approach is to create a template from an AWS EC2 instance. In this template, we can define our requirements using UserData scripts, which are shell scripts.

Let's learn all these concepts.

## Creating Templates in AWS EC2

### **Introduction**

In the dynamic landscape of cloud computing, efficiency is key. AWS EC2 (Elastic Compute Cloud) instances serve as the backbone of countless applications, and optimizing their deployment is crucial. In this blog post, we'll explore the concept of creating templates in AWS EC2—a powerful technique that enhances scalability, consistency, and ease of management.

### **Understanding the Need for Templates**

When managing multiple instances with similar configurations, the traditional approach of manually configuring each instance becomes time-consuming and error-prone. This is where templates come into play. A template, often referred to as an Amazon Machine Image (AMI), allows you to encapsulate a pre-configured instance and replicate it effortlessly.

### **Step-by-Step Guide: Creating an AWS EC2 Template**

1. **Launch an EC2 Instance:**
    
    Begin by launching an EC2 instance with the desired configuration, including the operating system, software, and settings.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701856193379/428ca415-1fa7-4206-b995-dc71eace28d1.png align="center")
    
    We launched a Ubuntu EC2 machine above.
    
2. **Customize the Instance:**
    
    Log in to the instance and make any necessary customizations. Install software, configure services, and ensure the instance meets your requirements.
    
    ```bash
    ubuntu@ip-172-31-41-162:~$ sudo apt-get update -y
    Hit:1 http://us-east-2.ec2.archive.ubuntu.com/ubuntu jammy InRelease
    Hit:2 http://us-east-2.ec2.archive.ubuntu.com/ubuntu jammy-updates InRelease
    Hit:3 http://us-east-2.ec2.archive.ubuntu.com/ubuntu jammy-backports InRelease
    Get:4 http://security.ubuntu.com/ubuntu jammy-security InRelease [110 kB]
    Fetched 110 kB in 1s (158 kB/s)                        
    Reading package lists... Done
    ubuntu@ip-172-31-41-162:~$ 
    ubuntu@ip-172-31-41-162:~$ sudo apt-get install nginx -y
    Reading package lists... Done
    Building dependency tree... Done
    Reading state information... Done
    nginx is already the newest version (1.18.0-6ubuntu14.4).
    0 upgraded, 0 newly installed, 0 to remove and 71 not upgraded.
    ubuntu@ip-172-31-41-162:~$ \
    > ^C
    ubuntu@ip-172-31-41-162:~$ cd /var/www/html/
    ubuntu@ip-172-31-41-162:/var/www/html$ ls
    index.nginx-debian.html
    ubuntu@ip-172-31-41-162:/var/www/html$ rm -rf *
    rm: cannot remove 'index.nginx-debian.html': Permission denied
    ubuntu@ip-172-31-41-162:/var/www/html$ sudo rm -rf *
    ubuntu@ip-172-31-41-162:/var/www/html$ sudo bi index.html
    sudo: bi: command not found
    ubuntu@ip-172-31-41-162:/var/www/html$ sudo vi index.html
    ubuntu@ip-172-31-41-162:/var/www/html$ cat index.html 
    Hello Techies!!!! We are learning AWS 
    ubuntu@ip-172-31-41-162:/var/www/html$ 
    ubuntu@ip-172-31-41-162:/var/www/html$ sudo systemctl restart nginx
    ubuntu@ip-172-31-41-162:/var/www/html$ 
    ubuntu@ip-172-31-41-162:/var/www/html$ sudo systemctl status nginx
    ● nginx.service - A high performance web server and a reverse proxy server
         Loaded: loaded (/lib/systemd/system/nginx.service; enabled; vendor preset: enabled)
         Active: active (running) since Wed 2023-12-06 10:08:04 UTC; 7s ago
           Docs: man:nginx(8)
        Process: 3810 ExecStartPre=/usr/sbin/nginx -t -q -g daemon on; master_process on; (code=exited, status=0/SUCCESS)
        Process: 3812 ExecStart=/usr/sbin/nginx -g daemon on; master_process on; (code=exited, status=0/SUCCESS)
       Main PID: 3813 (nginx)
          Tasks: 2 (limit: 1121)
         Memory: 2.5M
            CPU: 26ms
         CGroup: /system.slice/nginx.service
                 ├─3813 "nginx: master process /usr/sbin/nginx -g daemon on; master_process on;"
                 └─3814 "nginx: worker process" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" ""
    
    Dec 06 10:08:04 ip-172-31-41-162 systemd[1]: Starting A high performance web server and a reverse proxy server...
    Dec 06 10:08:04 ip-172-31-41-162 systemd[1]: Started A high performance web server and a reverse proxy server.
    ubuntu@ip-172-31-41-162:/var/www/html$
    ```
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701857415988/85d0d143-894b-4d23-9e05-24314e3e78c7.png align="center")
    
    We configured the Nginx server, and we created an `index.html` file, which is displayed above in the image.
    
    **Note** : *You can't get all these softwares in your template because we have done these installations after EC2 creation.*
    
3. **Prepare the Instance for Template Creation:**
    
    Before creating the template, ensure that the instance is in a clean and consistent state. Remove any sensitive information, clear logs, and reset temporary files.
    
4. **Access AWS Management Console:**
    
    Navigate to the AWS Management Console and go to the EC2 dashboard.
    
5. **Select the Instance:**
    
    In the Instances section, select the instance you want to use as a template.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701857671361/8c2b3ee9-8f87-4dea-b2a8-6bd23f0b338b.png align="center")
    
6. **Create Template:**
    
    1. Click on the "Actions" button, navigate to "Image and templates," and select "Create template from instance."
        
    2. Follow the on-screen instructions to create a template.
        
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701857884150/268224c7-ee0e-452b-90d1-2c9c6b6c301e.png align="center")
    
7. **Define Template Details:**
    
    Provide a meaningful name and description for your Template. Consider adding version information to facilitate future updates.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701858001818/01836005-c2f5-43b4-9cc5-0fc24724f7cf.png align="center")
    
    Provide a name for your launch template in the "Launch Template Name" field. Additionally, add a version description in the "Template version description" field.
    
8. Review all the remaining configurations, leave as it is.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701858114003/0c7b7d84-eecc-44ef-9880-40cfd7ee6bc1.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701858142398/a6cbd265-23e5-4b31-b81c-8f95569da922.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701858178502/6d57dfd1-b36b-4250-b3e4-ad2b87d93bae.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701858218905/70833420-fbb1-4962-8f22-5ab0b1c40ca2.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701858235168/1b66aa6d-dd51-4e83-9069-453eb2fa821a.png align="center")
    
9. **Create Launch Instance:**
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701858283315/fbd4c7b0-bbaf-4043-941f-2287e5e36146.png align="center")
    
10. **Check your Launch templates:**
    
    Monitor the template creation progress in the AWS Management Console. Once completed, your template is ready for use.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701858355449/4349c5b4-4da5-4d8d-8375-a2f4cfffd775.png align="center")
    

Now that our template is ready, we can deploy any number of EC2 instances from this template in one go.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701926096349/33639e71-67c3-4661-8c40-17b2a9a0205e.png align="center")

Select your template from the "Launch Templates" menu. Go to "Actions," and choose "Launch Instances from the template." Additionally, you can modify the existing template by creating a newer version and incorporate user data scripts into it.

Let's proceed with the template modification task by adding user data scripts.

### **Modify the Template by adding User data Scripts:**

Let's understand first, what is User Data Scripts?

**User Data Scripts:**

User data scripts in AWS EC2 are essentially shell scripts or commands that can be passed to an EC2 instance during its launch. These scripts automate tasks or configurations that need to be performed when the instance boots up.

User data scripts allow you to:

1. **Configure Instances:** They help in setting up an instance as per your requirements. For instance, installing software packages, configuring settings, or performing any necessary actions on startup.
    
2. **Automation:** Automate repetitive tasks that you would otherwise need to do manually after launching an instance.
    
3. **Customization:** Tailor the behavior of your instances based on specific needs by defining these scripts.
    

The user data can be provided in plain text or as a shell script. AWS EC2 executes this user data script when the instance is first launched. It's a one-time execution, typically used for tasks such as configuring the instance as a web server, setting up databases, initiating backups, and more.

For example, you might use a user data script to install and start a web server like Apache or Nginx, set up monitoring agents, download and configure applications, or pull configuration files from an external source.

The user data script runs with root privileges, enabling it to execute administrative tasks during the initial boot process of the EC2 instance, saving time and effort in manual configurations after launch.

### Steps for providing User Data Scripts:

In this practical, we will use the template created earlier and proceed to modify it.

1. To modify the template, select the template from Launch Templates, go to Actions, and choose "Modify Template (Create new version)."
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701926567466/302b069c-f95f-439b-b1b3-f36239d4d5ea.png align="center")
    
2. Provide a new version name for your modified template.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701926639449/29b64b1a-62ea-4ba1-909f-6eea1ef5aa6e.png align="center")
    
3. Leave all the options as they are, including AMI, Instance Type, Key Pair, Network Settings, Storage, and Resource tags. We will make modifications in Advanced details.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701926763152/2d8b7668-a3b2-4f33-a8c3-016985b778e7.png align="center")
    
    Scroll down to the advanced options. At the bottom, you will find the "User data" option. Here, you can write the script that you want to run during EC2 creation.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701926851942/059d5b2f-5dc1-4187-b5b0-4b1f60ced429.png align="center")
    
4. Enter your shell script in the "User data" option:
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701939673498/70dc6b40-6d25-45ab-a7cd-ed272b840220.png align="center")
    
    After writing your code, select "Create template version"
    
5. Now, we have two versions of our template. The first version involves launching an Ubuntu instance, configuring Nginx after launch, and creating a template from that configuration. The second version is a modification of the existing template, incorporating user data scripts.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701934279754/0d67a3c2-6733-4765-965b-73aec33d8ae1.png align="center")
    
    Here, we can observe that the default version is 1, and the latest version is 2. It's now your choice to select the template version based on your specific use case.
    

### Conclusion

As we conclude this walkthrough, we've successfully navigated the process of creating and modifying AWS EC2 templates. From launching instances and configuring them to the advanced step of incorporating user data scripts, these templates serve as powerful tools for streamlining and automating the deployment of instances. Whether you choose the initial version or the modified one, the flexibility offered by templates empowers you to tailor your instances to meet the demands of your applications. Happy computing in the cloud!