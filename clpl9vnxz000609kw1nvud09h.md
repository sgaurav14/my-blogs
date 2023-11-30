---
title: "Mastering Jenkins: A Beginner's Guide to Automation Excellence"
datePublished: Thu Nov 30 2023 14:09:48 GMT+0000 (Coordinated Universal Time)
cuid: clpl9vnxz000609kw1nvud09h
slug: mastering-jenkins-a-beginners-guide-to-automation-excellence
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1701353112943/f1425184-28a9-4512-be53-bbf017da5be8.webp
tags: jenkins, jenkins-devops, 90daysofdevops, jenkins-installation

---

In the fast-paced world of software development, streamlining processes is key to staying ahead. Imagine having a digital assistant that automates tedious tasks, allowing your team to focus on innovation. This is where Jenkins steps in – an open-source automation server that acts as the backbone of efficient software development. In this blog, we'll demystify Jenkins in a non-technical way, exploring its significance and how it enhances workflows.

### Understanding Jenkins:

Jenkins is essentially your digital helper, automating tasks like building, testing, and deploying software projects. Don't let the technical jargon intimidate you; instead, think of Jenkins as the secret ingredient that makes software development smoother and more efficient.

Essential Concepts:

1. **Continuous Integration (CI):** Jenkins promotes Continuous Integration, a practice where developers regularly merge their code changes into a shared repository. Jenkins then kicks off automated processes to ensure the new code doesn't disrupt the existing project.
    
2. **Pipeline:** Picture a Jenkins pipeline as a visual roadmap of your software delivery process. It outlines the automated steps involved in building, testing, and deploying code, providing a clear overview of the development journey.
    
3. **Plugins:** Jenkins becomes a versatile tool through plugins. These are like apps that extend Jenkins' capabilities by integrating it with various tools and technologies. Think of them as the plugins on your favorite mobile app – they add functionality without complicating things.
    

Why Jenkins Matters:

1. **Efficiency Boost:** Jenkins takes care of repetitive tasks, saving time and effort. This efficiency translates to quicker development cycles and faster project delivery.
    
2. **Reliability:** Automation reduces the risk of human error. Jenkins ensures consistency in the development process, resulting in more reliable and stable software.
    
3. **Cost Savings:** By automating manual processes, Jenkins not only saves time but also cuts down on development costs. It's like having a diligent assistant that works tirelessly without incurring overtime expenses.
    

How Jenkins Works (in Layman's Terms):

Think of Jenkins as your project manager overseeing the development highway. Here's a simple breakdown:

1. **Code Commit:** Developers make changes to the code and save them in a shared repository.
    
2. **Jenkins Trigger:** Jenkins keeps an eye on the repository. When it spots a code change, it initiates a set of actions.
    
3. **Build and Test:** Jenkins starts the build process, compiling the code and running tests to ensure it works correctly.
    
4. **Deployment:** If the build and tests pass, Jenkins can automatically launch the code in the desired environment.
    

## **Installation of Jenkins**

We are doing the installation of Jenkins on an Oracle Linux 7 machine.

### Prerequisites:

**Minimum hardware requirements:**

* 256 MB of RAM
    
* 1 GB of drive space (although 10 GB is a recommended minimum if running Jenkins as a Docker container)
    

**Software requirements:**

* Java
    
* fontconfig package
    

### Steps:

1. Before diving into the Jenkins installation, let's ensure that your system is equipped with Java 11. Follow these steps to install it:
    
    ```bash
    [sid@sid-vm ~]$ yum install java-11-openjdk-devel
    Loaded plugins: langpacks, ulninfo
    You need to be root to perform this command.
    [sid@sid-vm ~]$ sudo yum install java-11-openjdk-devel
    Loaded plugins: langpacks, ulninfo
    Resolving Dependencies
    --> Running transaction check
    ---> Package java-11-openjdk-devel.x86_64 1:11.0.21.0.9-1.0.1.el7_9 will be installed
    --> Processing Dependency: java-11-openjdk(x86-64) = 1:11.0.21.0.9-1.0.1.el7_9 for package: 1:java-11-openjdk-devel-11.0.21.0.9-1.0.1.el7_9.x86_64
    --> Running transaction check
    ---> Package java-11-openjdk.x86_64 1:11.0.21.0.9-1.0.1.el7_9 will be installed
    --> Finished Dependency Resolution
    
    Dependencies Resolved
    
    ==========================================================================================================================================
     Package                               Arch                   Version                                    Repository                  Size
    ==========================================================================================================================================
    Installing:
     java-11-openjdk-devel                 x86_64                 1:11.0.21.0.9-1.0.1.el7_9                  ol7_latest                 3.4 M
    Installing for dependencies:
     java-11-openjdk                       x86_64                 1:11.0.21.0.9-1.0.1.el7_9                  ol7_latest                 242 k
    
    Transaction Summary
    ==========================================================================================================================================
    Install  1 Package (+1 Dependent package)
    
    Total download size: 3.6 M
    Installed size: 5.7 M
    Is this ok [y/d/N]: y
    Downloading packages:
    (1/2): java-11-openjdk-11.0.21.0.9-1.0.1.el7_9.x86_64.rpm                                                          | 242 kB  00:00:00
    (2/2): java-11-openjdk-devel-11.0.21.0.9-1.0.1.el7_9.x86_64.rpm                                                    | 3.4 MB  00:00:01
    ------------------------------------------------------------------------------------------------------------------------------------------
    Total                                                                                                     2.3 MB/s | 3.6 MB  00:00:01
    Running transaction check
    Running transaction test
    Transaction test succeeded
    Running transaction
      Installing : 1:java-11-openjdk-11.0.21.0.9-1.0.1.el7_9.x86_64                                                                       1/2
      Installing : 1:java-11-openjdk-devel-11.0.21.0.9-1.0.1.el7_9.x86_64                                                                 2/2
      Verifying  : 1:java-11-openjdk-11.0.21.0.9-1.0.1.el7_9.x86_64                                                                       1/2
      Verifying  : 1:java-11-openjdk-devel-11.0.21.0.9-1.0.1.el7_9.x86_64                                                                 2/2
    
    Installed:
      java-11-openjdk-devel.x86_64 1:11.0.21.0.9-1.0.1.el7_9
    
    Dependency Installed:
      java-11-openjdk.x86_64 1:11.0.21.0.9-1.0.1.el7_9
    
    Complete!
    [sid@sid-vm ~]$
    ```
    
2. The Java installation is complete. Now, let's verify the version.
    
    ```bash
    [sid@sid-vm ~]$ java --version
    openjdk 11.0.21 2023-10-17 LTS
    OpenJDK Runtime Environment (Red_Hat-11.0.21.0.9-1.0.1.el7_9) (build 11.0.21+9-LTS)
    OpenJDK 64-Bit Server VM (Red_Hat-11.0.21.0.9-1.0.1.el7_9) (build 11.0.21+9-LTS, mixed mode, sharing)
    [sid@sid-vm ~]$
    ```
    
3. Let's also install fontconfig.
    
    ```bash
    [sid@sid-vm ~]$ sudo yum install fontconfig
    Loaded plugins: langpacks, ulninfo
    Package fontconfig-2.13.0-4.3.el7.x86_64 already installed and latest version
    Nothing to do
    [sid@sid-vm ~]$
    ```
    
4. Now it's time to obtain the Jenkins repository.
    
    ```bash
    [sid@sid-vm ~]$ sudo wget -O /etc/yum.repos.d/jenkins.repo \
    >     https://pkg.jenkins.io/redhat-stable/jenkins.repo
    --2023-11-30 07:09:36--  https://pkg.jenkins.io/redhat-stable/jenkins.repo
    Resolving pkg.jenkins.io (pkg.jenkins.io)... 199.232.22.133
    Connecting to pkg.jenkins.io (pkg.jenkins.io)|199.232.22.133|:443... connected.
    HTTP request sent, awaiting response... 200 OK
    Length: 85
    Saving to: ‘/etc/yum.repos.d/jenkins.repo’
    
    100%[================================================================================================>] 85          --.-K/s   in 0s
    
    2023-11-30 07:09:37 (1.16 MB/s) - ‘/etc/yum.repos.d/jenkins.repo’ saved [85/85]
    
    [sid@sid-vm ~]$
    ```
    
5. Jenkins requires a key for validation to verify its actual configuration repository. Let's download that key.
    
    ```bash
    [sid@sid-vm ~]$ sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
    [sid@sid-vm ~]$ 
    ```
    
6. Let's proceed with upgrading the system.
    
    ```bash
    [sid@sid-vm ~]$ sudo yum upgrade
    ```
    
7. Now it's time to proceed with the actual installation of the Jenkins package.
    
    ```bash
    [sid@sid-vm ~]$ sudo yum install jenkins
    Loaded plugins: langpacks, ulninfo
    Resolving Dependencies
    --> Running transaction check
    ---> Package jenkins.noarch 0:2.426.1-1.1 will be installed
    --> Finished Dependency Resolution
    
    Dependencies Resolved
    
    ==========================================================================================================================================
     Package                         Arch                           Version                             Repository                       Size
    ==========================================================================================================================================
    Installing:
     jenkins                         noarch                         2.426.1-1.1                         jenkins                          85 M
    
    Transaction Summary
    ==========================================================================================================================================
    Install  1 Package
    
    Total download size: 85 M
    Installed size: 85 M
    Is this ok [y/d/N]: y
    Downloading packages:
    jenkins-2.426.1-1.1.noarch.rpm                                                                                     |  85 MB  00:00:21
    Running transaction check
    Running transaction test
    Transaction test succeeded
    Running transaction
      Installing : jenkins-2.426.1-1.1.noarch                                                                                             1/1
      Verifying  : jenkins-2.426.1-1.1.noarch                                                                                             1/1
    
    Installed:
      jenkins.noarch 0:2.426.1-1.1
    
    Complete!
    [sid@sid-vm ~]$
    ```
    
8. Now that the package is installed, start the Jenkins service and enable it to ensure it starts up after every reboot.
    
    ```bash
    [sid@sid-vm ~]$ sudo systemctl start jenkins
    [sid@sid-vm ~]$ sudo systemctl enable jenkins
    [sid@sid-vm ~]$
    [sid@sid-vm ~]$ sudo systemctl status jenkins
    ● jenkins.service - Jenkins Continuous Integration Server
       Loaded: loaded (/usr/lib/systemd/system/jenkins.service; enabled; vendor preset: disabled)
       Active: active (running) since Thu 2023-11-30 08:27:01 EST; 14s ago
     Main PID: 13060 (java)
       CGroup: /system.slice/jenkins.service
               └─13060 /usr/bin/java -Djava.awt.headless=true -jar /usr/share/java/jenkins.war --webroot=%C/jenkins/war --httpPort=8080
    
    Nov 30 08:26:35 sid-vm.ha.stalab.ciena.com jenkins[13060]: *************************************************************
    Nov 30 08:26:35 sid-vm.ha.stalab.ciena.com jenkins[13060]: *************************************************************
    Nov 30 08:26:36 sid-vm.ha.stalab.ciena.com jenkins[13060]: WARNING: An illegal reflective access operation has occurred
    Nov 30 08:26:36 sid-vm.ha.stalab.ciena.com jenkins[13060]: WARNING: Illegal reflective access by org.codehaus.groovy.vmplugin.v7.J...,int)
    Nov 30 08:26:36 sid-vm.ha.stalab.ciena.com jenkins[13060]: WARNING: Please consider reporting this to the maintainers of org.codeh...va7$1
    Nov 30 08:26:36 sid-vm.ha.stalab.ciena.com jenkins[13060]: WARNING: Use --illegal-access=warn to enable warnings of further illega...tions
    Nov 30 08:26:36 sid-vm.ha.stalab.ciena.com jenkins[13060]: WARNING: All illegal access operations will be denied in a future release
    Nov 30 08:27:01 sid-vm.ha.stalab.ciena.com jenkins[13060]: 2023-11-30 13:27:01.951+0000 [id=32]        INFO        jenkins.InitRea...ation
    Nov 30 08:27:01 sid-vm.ha.stalab.ciena.com jenkins[13060]: 2023-11-30 13:27:01.996+0000 [id=23]        INFO        hudson.lifecycl...nning
    Nov 30 08:27:01 sid-vm.ha.stalab.ciena.com systemd[1]: Started Jenkins Continuous Integration Server.
    Hint: Some lines were ellipsized, use -l to show in full.
    [sid@sid-vm ~]$
    ```
    
9. Let's access the Jenkins dashboard by opening http://&lt;IP&gt;:8080.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701350908794/f00023b7-55ea-4945-8e13-e6376159ec01.png align="center")
    
10. It is requesting the Administrator password. View the password from the /var/lib/jenkins/secrets/initialAdminPassword file using the `cat` command, and paste the output on the screen as prompted.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701351206660/9830ae19-1563-4afb-b31c-25a67cb69df6.png align="center")
    
    After entering the password, click 'Continue'.
    
11. Afterwards, it will prompt you to install plugins on your Jenkins dashboard. You have two options: either go with the suggested plugins or manually select and install them.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701351364567/badb73ea-e753-4ed5-944c-ccdace97e1d0.png align="center")
    
    We'll opt for the 'Install suggested plugins' option. Simply click on that choice, and it will proceed to install all the suggested plugins.
    
12. Now, it will prompt you to create an admin user. Create the user by filling out the template.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701351626887/ce243edc-2f25-4908-8a84-03613bd87c52.png align="center")
    
    Click on 'Save and Continue'.
    
13. At the end, it will prompt you to set up your Jenkins URL. Configure it accordingly.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701351698029/2e3b9307-b39c-4f3e-8ca2-072216cf5245.png align="center")
    
    Click on 'Save and Finish'.
    
14. Your Jenkins installation is complete. You will receive the following completion message:
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701351787461/61259c89-8248-474e-be1e-4a60687723c4.png align="center")
    
    Click on "Start using Jenkins".
    
15. Hurray! We finally have our Jenkins dashboard ready to automate our tasks.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701351895774/620f18c7-f8dd-4f81-9e75-b85326888df0.png align="center")
    

You can now proceed to create your jobs.

**Certainly! After setting up Jenkins, here's a simplified guide:**

1. **Explore Jenkins:** Start by exploring Jenkins features. Learn to create and set up jobs, build pipelines, and use plugins.
    
2. **Check Documentation:** Refer to the official Jenkins documentation for detailed information. Look for tutorials and resources to enhance your skills.
    
3. **Learn and Train:** Consider online courses or training programs to become more proficient in Jenkins and continuous integration.
    
4. **Join Community:** Connect with the Jenkins community through forums or mailing lists. Share experiences and get support from others.
    
5. **Integrate with Git:** Integrate Jenkins with Git or other version control systems for a seamless workflow.
    
6. **Scheduled Builds:** Set up scheduled builds for automation. Configure notifications to keep the team informed of build statuses.
    
7. **Security First:** Ensure user authentication, authorization, and secure plugin usage to keep your Jenkins instance safe.
    
8. **Backup and Recovery:** Regularly back up Jenkins configurations and data for a secure setup.
    
9. **Scale as Needed:** Explore options for scaling Jenkins based on project or organization size. Consider distributed setups or using Jenkins agents.
    
10. **Stay Updated:** Keep Jenkins and plugins updated to benefit from new features, bug fixes, and security patches.
    

This concise guide provides a roadmap for further learning and exploration, helping users make the most of Jenkins in their development workflows.