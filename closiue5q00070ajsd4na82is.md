---
title: "Linux Unveiled: A Beginner's Tour Through Packages, Managers, and System Helpers"
datePublished: Fri Nov 10 2023 11:15:26 GMT+0000 (Coordinated Universal Time)
cuid: closiue5q00070ajsd4na82is
slug: linux-unveiled-a-beginners-tour-through-packages-managers-and-system-helpers
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1699548264437/b1629620-6f81-40b0-9dd8-601f23edf3ef.avif
tags: linux, package-manager, linux-for-beginners, linux-basics

---

### Introduction

Welcome to the fascinating world of Linux! If you're new to this operating system, you might have heard terms like "package manager," "systemctl," and "systemd." In this simple guide, we'll explore these concepts in a way that even those new to tech can understand.

**Understanding Packages:** Imagine you want to use a new app on your phone. To make it work, you need not only the app itself but also all the necessary files and settings. In the Linux world, we call this bundle of everything a "package." It's like getting a gift box that contains everything needed to make your app run smoothly.

**Package Managers:** Now, think of a package manager as your personal assistant who takes care of installing, updating, and removing software on your computer. You tell them what you want, and they handle the rest. Let's look at different ones:

1. **APT (Advanced Package Tool):**
    
    * Think of it like a helpful friend for systems like Ubuntu.
        
    
    Example: To get a new tool called "VSCode," you'd simply tell your friend, "Get me VSCode," and they'll make it happen.
    
    ```bash
    sudo apt-get install -y docker-ce
    ```
    
2. **YUM (Yellowdog Updater, Modified):**
    
    * This buddy is for systems like CentOS and Fedora.
        
    
    Example: If you want a container server called docker, just say, "I need Docker," and YUM will make it ready for you.
    
    ```bash
    [sid@sid-vm ~]$ sudo yum install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
    Loaded plugins: langpacks, ulninfo
    Resolving Dependencies
    --> Running transaction check
    ---> Package containerd.io.x86_64 0:1.6.24-3.1.el7 will be installed
    ---> Package docker-buildx-plugin.x86_64 0:0.11.2-1.el7 will be installed
    ---> Package docker-ce.x86_64 3:24.0.7-1.el7 will be installed
    --> Processing Dependency: docker-ce-rootless-extras for package: 3:docker-ce-24.0.7-1.el7.x86_64
    ---> Package docker-ce-cli.x86_64 1:24.0.7-1.el7 will be installed
    ---> Package docker-compose-plugin.x86_64 0:2.21.0-1.el7 will be installed
    --> Running transaction check
    ---> Package docker-ce-rootless-extras.x86_64 0:24.0.7-1.el7 will be installed
    --> Finished Dependency Resolution
    
    Dependencies Resolved
    
    ====================================================================================================================================
     Package                                  Arch                  Version                       Repository                       Size
    ====================================================================================================================================
    Installing:
     containerd.io                            x86_64                1.6.24-3.1.el7                docker-ce-stable                 34 M
     docker-buildx-plugin                     x86_64                0.11.2-1.el7                  docker-ce-stable                 13 M
     docker-ce                                x86_64                3:24.0.7-1.el7                docker-ce-stable                 24 M
     docker-ce-cli                            x86_64                1:24.0.7-1.el7                docker-ce-stable                 13 M
     docker-compose-plugin                    x86_64                2.21.0-1.el7                  docker-ce-stable                 13 M
    Installing for dependencies:
     docker-ce-rootless-extras                x86_64                24.0.7-1.el7                  docker-ce-stable                9.1 M
    
    Transaction Summary
    ====================================================================================================================================
    Install  5 Packages (+1 Dependent package)
    
    Total download size: 107 M
    Installed size: 377 M
    Is this ok [y/d/N]: y
    Downloading packages:
    warning: /var/cache/yum/x86_64/7Server/docker-ce-stable/packages/docker-buildx-plugin-0.11.2-1.el7.x86_64.rpm: Header V4 RSA/SHA512 Signature, key ID 621e9f35: NOKEY
    Public key for docker-buildx-plugin-0.11.2-1.el7.x86_64.rpm is not installed
    (1/6): docker-buildx-plugin-0.11.2-1.el7.x86_64.rpm                                                          |  13 MB  00:00:02
    (2/6): containerd.io-1.6.24-3.1.el7.x86_64.rpm                                                               |  34 MB  00:00:04
    (3/6): docker-ce-24.0.7-1.el7.x86_64.rpm                                                                     |  24 MB  00:00:02
    (4/6): docker-ce-rootless-extras-24.0.7-1.el7.x86_64.rpm                                                     | 9.1 MB  00:00:01
    (5/6): docker-ce-cli-24.0.7-1.el7.x86_64.rpm                                                                 |  13 MB  00:00:01
    (6/6): docker-compose-plugin-2.21.0-1.el7.x86_64.rpm                                                         |  13 MB  00:00:01
    ------------------------------------------------------------------------------------------------------------------------------------
    Total                                                                                                12 MB/s | 107 MB  00:00:08
    Retrieving key from https://download.docker.com/linux/centos/gpg
    Importing GPG key 0x621E9F35:
     Userid     : "Docker Release (CE rpm) <docker@docker.com>"
     Fingerprint: 060a 61c5 1b55 8a7f 742b 77aa c52f eb6b 621e 9f35
     From       : https://download.docker.com/linux/centos/gpg
    Is this ok [y/N]: y
    Running transaction check
    Running transaction test
    Transaction test succeeded
    Running transaction
      Installing : docker-buildx-plugin-0.11.2-1.el7.x86_64                                                                         1/6
      Installing : docker-compose-plugin-2.21.0-1.el7.x86_64                                                                        2/6
      Installing : 1:docker-ce-cli-24.0.7-1.el7.x86_64                                                                              3/6
      Installing : containerd.io-1.6.24-3.1.el7.x86_64                                                                              4/6
      Installing : docker-ce-rootless-extras-24.0.7-1.el7.x86_64                                                                    5/6
      Installing : 3:docker-ce-24.0.7-1.el7.x86_64                                                                                  6/6
      Verifying  : 3:docker-ce-24.0.7-1.el7.x86_64                                                                                  1/6
      Verifying  : docker-ce-rootless-extras-24.0.7-1.el7.x86_64                                                                    2/6
      Verifying  : containerd.io-1.6.24-3.1.el7.x86_64                                                                              3/6
      Verifying  : docker-compose-plugin-2.21.0-1.el7.x86_64                                                                        4/6
      Verifying  : 1:docker-ce-cli-24.0.7-1.el7.x86_64                                                                              5/6
      Verifying  : docker-buildx-plugin-0.11.2-1.el7.x86_64                                                                         6/6
    
    Installed:
      containerd.io.x86_64 0:1.6.24-3.1.el7      docker-buildx-plugin.x86_64 0:0.11.2-1.el7       docker-ce.x86_64 3:24.0.7-1.el7
      docker-ce-cli.x86_64 1:24.0.7-1.el7        docker-compose-plugin.x86_64 0:2.21.0-1.el7
    
    Dependency Installed:
      docker-ce-rootless-extras.x86_64 0:24.0.7-1.el7
    
    Complete!
    [sid@sid-vm ~]$
    ```
    

Your friendly assistants make sure you can use your computer without worrying about technical details.

### **Systemd and systemctl:**

Now, let's talk about two more pals, systemd and systemctl. They help keep everything running smoothly on your system.

1. **Systemd:**
    
    * Picture systemd as the guardian of your computer's well-being.
        
    
    Example: When you start your computer, systemd makes sure everything wakes up in the right order, like your morning routine.
    
2. **systemctl:**
    
    * systemctl is like your remote control for managing what's happening on your computer.
        
    
    Example: Want to start a service, like a docker engine server? Just press a button (or type a command), and systemctl takes care of it.
    

* **To check the docker service:**
    

```bash
[sid@sid-vm ~]$ sudo systemctl status docker
● docker.service - Docker Application Container Engine
   Loaded: loaded (/usr/lib/systemd/system/docker.service; disabled; vendor preset: disabled)
   Active: inactive (dead)
     Docs: https://docs.docker.com
[sid@sid-vm ~]$
[sid@sid-vm ~]$
```

* **To start the Docker service:**
    

```bash
[sid@sid-vm ~]$ sudo systemctl start docker
```

* **Check the service now after starting the service:**
    

```bash
[sid@sid-vm ~]$ sudo systemctl status docker
● docker.service - Docker Application Container Engine
   Loaded: loaded (/usr/lib/systemd/system/docker.service; disabled; vendor preset: disabled)
   Active: active (running) since Thu 2023-11-09 11:30:51 EST; 4s ago
     Docs: https://docs.docker.com
 Main PID: 23191 (dockerd)
    Tasks: 10
   Memory: 28.3M
   CGroup: /system.slice/docker.service
           └─23191 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock

Nov 09 11:30:48 sid-vm.ha.stalab.ciena.com systemd[1]: Starting Docker Application Container Engine...
Nov 09 11:30:48 sid-vm.ha.stalab.ciena.com dockerd[23191]: time="2023-11-09T11:30:48.743682488-05:00" level=info msg="Starting up"
Nov 09 11:30:48 sid-vm.ha.stalab.ciena.com dockerd[23191]: time="2023-11-09T11:30:48.888590461-05:00" level=info msg="Loadin...art."
Nov 09 11:30:51 sid-vm.ha.stalab.ciena.com dockerd[23191]: time="2023-11-09T11:30:51.398941090-05:00" level=info msg="Firewa...ning"
Nov 09 11:30:51 sid-vm.ha.stalab.ciena.com dockerd[23191]: time="2023-11-09T11:30:51.721148619-05:00" level=info msg="Loadin...one."
Nov 09 11:30:51 sid-vm.ha.stalab.ciena.com dockerd[23191]: time="2023-11-09T11:30:51.783372307-05:00" level=warning msg="Not...rlay2
Nov 09 11:30:51 sid-vm.ha.stalab.ciena.com dockerd[23191]: time="2023-11-09T11:30:51.784215939-05:00" level=info msg="Docker...4.0.7
Nov 09 11:30:51 sid-vm.ha.stalab.ciena.com dockerd[23191]: time="2023-11-09T11:30:51.784909943-05:00" level=info msg="Daemon...tion"
Nov 09 11:30:51 sid-vm.ha.stalab.ciena.com dockerd[23191]: time="2023-11-09T11:30:51.950260256-05:00" level=info msg="API li...sock"
Nov 09 11:30:51 sid-vm.ha.stalab.ciena.com systemd[1]: Started Docker Application Container Engine.
Hint: Some lines were ellipsized, use -l to show in full.
[sid@sid-vm ~]$
```

* **Stopping the Docker service:**
    

```bash
[sid@sid-vm ~]$ sudo systemctl stop docker
Warning: Stopping docker.service, but it can still be activated by:
  docker.socket
[sid@sid-vm ~]$
[sid@sid-vm ~]$
```

* **Restarting the Docker service:**
    

```bash
[sid@sid-vm ~]$
[sid@sid-vm ~]$ sudo systemctl restart docker
[sid@sid-vm ~]$
```

**Conclusion:** In this little journey, we've introduced you to your computer's helpers—the package managers, systemd, and systemctl. They're like friends making sure your computer experience is smooth and enjoyable. So, don't be afraid to explore and enjoy your time in the Linux world! Happy adventures!