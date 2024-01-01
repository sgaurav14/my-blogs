---
title: "A Beginner's Guide to Linux Network and Network Management: Demystifying the Basics"
datePublished: Mon Jan 01 2024 16:48:20 GMT+0000 (Coordinated Universal Time)
cuid: clqv5mt6q000508l6ew2f8z1v
slug: a-beginners-guide-to-linux-network-and-network-management-demystifying-the-basics
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1704127642435/9f3c3837-13e7-4301-90ba-6f3343e85588.png

---

In the world of technology, understanding the fundamentals of networking is becoming increasingly essential. Linux, as a powerful and widely-used operating system, plays a significant role in this landscape. If you're new to the world of networks and don't consider yourself a tech guru, fear not! This blog aims to demystify Linux networking for beginners and non-tech individuals, providing a simple and approachable introduction.

Understanding the Basics:

1. **What is a Network?**
    
    At its core, a network is a collection of computers and devices connected to share resources and information. In the context of Linux, this could be multiple computers communicating over the same network.
    
2. **Linux and Networking: A Natural Pairing**
    
    Linux, being open-source and highly customizable, is an excellent choice for networking. It powers a significant portion of servers on the internet, making it crucial to grasp some networking concepts when dealing with Linux systems.
    

**Getting Started:**

Let's go through some examples specifically tailored for CentOS or Red Hat Enterprise Linux (RHEL).

### **1\. Understanding IP Addresses:**

**Viewing IP Address:**

```bash
ip addr
```

or

```bash
ifconfig
```

* Look for the IP address under your network interface (`eth0`, `enp0s3`, or similar).
    

### **2\. Connectivity and Ping:**

**Ping Command:**

```bash
ping 8.8.8.8
```

* Checks if your machine can communicate with the Google Public DNS server.
    

### **3\. Network Configuration Files:**

**Editing** `/etc/sysconfig/network-scripts/ifcfg-eth0` **(for Ethernet) or** `/etc/sysconfig/network-scripts/ifcfg-wlan0` (for Wireless):

```bash
sudo vim /etc/sysconfig/network-scripts/ifcfg-eth0
```

* Set a static IP address:
    
    ```bash
    DEVICE=eth0
    BOOTPROTO=none
    ONBOOT=yes
    IPADDR=192.168.1.10
    NETMASK=255.255.255.0
    GATEWAY=192.168.1.1
    ```
    
* Save the file and restart the network service:
    
    ```bash
    sudo systemctl restart network
    ```
    

### **4\. Wi-Fi Networks:**

**Connecting to Wi-Fi using** `nmtui`**:**

```bash
nmtui
```

* Use the arrow keys to navigate, select "Activate a connection," choose your Wi-Fi network, and enter the password.
    

### **5\. NetworkManager:**

**Using** `nmcli`**:**

```bash
nmcli connection show
```

* Lists available connections.
    

```bash
nmcli connection modify [connection-name] ipv4.method manual ipv4.addresses 192.168.1.10/24 ipv4.gateway 192.168.1.1
```

* Modifies a connection to use a static IP address.
    

### **6\. Firewall Basics using** `firewalld`**:**

**Enabling** `firewalld`**:**

```bash
sudo systemctl enable --now firewalld
```

**Allowing Ports:**

```bash
sudo firewall-cmd --zone=public --add-port=80/tcp --permanent
sudo firewall-cmd --reload
```

* Allows incoming traffic on port 80 (HTTP).
    

Remember to adapt these examples to your specific network configuration, and always exercise caution when making changes, especially on production systems.

### Conclusion:

By embracing the basics of Linux networking, you've taken a significant step toward understanding the digital world around you. Whether you're connecting to the internet, sharing files with a friend, or setting up a home server, these fundamental concepts will serve you well. Don't be afraid to explore further, as the world of Linux networking is vast and filled with exciting possibilities. Happy networking!