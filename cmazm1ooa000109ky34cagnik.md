---
title: "🌐 Understanding Subnetting: A Beginner's Guide with Simple Examples"
datePublished: Thu May 22 2025 16:52:59 GMT+0000 (Coordinated Universal Time)
cuid: cmazm1ooa000109ky34cagnik
slug: understanding-subnetting-a-beginners-guide-with-simple-examples
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1747932005266/864efd52-e0e7-4195-a570-b7689fec1fc5.jpeg
tags: aws, devops, networking, computer-network, computer-networking, networking-for-beginners, ip-address, subnetting

---

In our previous blog, we explored the fundamentals of IP addresses—what they are, how they work, and the difference between IPv4 and IPv6. Now, it’s time to dive into a more advanced but **extremely important concept in networking**: **Subnetting**.

Subnetting might sound complex at first, but I promise, by the end of this post, you’ll be comfortable with what it is, why we use it, and how it works—with easy-to-understand examples.

---

## 📘 What is Subnetting?

**Subnetting** is the process of dividing a large network into smaller, more manageable sub-networks (called *subnets*). Each subnet can function independently while still being part of the larger network.

Think of it like this: If a company owns a large office building, subnetting allows them to divide it into different departments—HR, Finance, IT—each with their own section and management, but still part of the same company.

---

## 🎯 Why Do We Use Subnetting?

* ✅ **Efficient IP Address Usage:** Helps prevent wastage of IP addresses.
    
* ✅ **Improved Network Performance:** Reduces network congestion by limiting broadcast traffic to smaller subnetworks.
    
* ✅ **Better Security & Management:** Allows isolation of different departments or teams in an organization.
    
* ✅ **Simplifies Troubleshooting:** Easier to identify and isolate issues within a subnet.
    

---

## 🧮 Before We Dive Deeper: A Quick Review of IP Addressing

Let’s recall an IPv4 address:

```bash
192.168.1.1
```

This address is made up of **32 bits**, usually written in **4 decimal octets** (each 8 bits):

```bash
Binary: 11000000.10101000.00000001.00000001
     =   192     . 168    .   1   .   1
```

In subnetting, we use something called a **subnet mask** to separate:

* The **network portion** of the address
    
* From the **host portion** (the devices)
    

---

## 🛠 What is a Subnet Mask?

A **subnet mask** is used to determine how many bits are reserved for the **network** and how many are available for **hosts**.

Common subnet masks:

* `255.0.0.0` → `/8`
    
* `255.255.0.0` → `/16`
    
* `255.255.255.0` → `/24`
    

For example:

```bash
IP Address:     192.168.1.10
Subnet Mask:    255.255.255.0
```

This means:

* First 24 bits are for the **network** (`/24`)
    
* Last 8 bits are for **hosts**
    

---

## ✂️ CIDR Notation

**CIDR** (Classless Inter-Domain Routing) uses a **slash** to represent how many bits are used for the network.

Example:  
`192.168.1.0/24` means the first 24 bits are network bits.

---

## 🧠 Let’s Understand Subnetting with a Simple Example

### 🎯 Goal: Create 4 subnets from `192.168.1.0/24`

`/24` means:

* 256 total IP addresses (2⁸)
    
* 254 usable (1 for network, 1 for broadcast)
    

We want 4 subnets → 2 bits needed (2² = 4)

So we borrow 2 bits from host portion:

```bash
Subnet Mask: /26 (24 + 2)
```

Each subnet will have:

* 64 IP addresses (2⁶)
    
* 62 usable addresses per subnet
    

### ✅ Resulting Subnets:

| Subnet | Network Address | Usable Range | Broadcast Address |
| --- | --- | --- | --- |
| 1 | 192.168.1.0/26 | 192.168.1.1–62 | 192.168.1.63 |
| 2 | 192.168.1.64/26 | 192.168.1.65–126 | 192.168.1.127 |
| 3 | 192.168.1.128/26 | 192.168.1.129–190 | 192.168.1.191 |
| 4 | 192.168.1.192/26 | 192.168.1.193–254 | 192.168.1.255 |

👏 Now you’ve divided one large network into 4 smaller networks!

---

## 🧾 How to Calculate Subnets (Step-by-Step)

1. **Determine your original subnet (CIDR)**  
    e.g., `/24` = 256 IPs
    
2. **Decide how many subnets you want**  
    e.g., 4 subnets → Need 2 extra bits
    
3. **Add bits to subnet mask**  
    `/24 + 2 = /26`
    
4. **Calculate number of hosts per subnet**  
    2⁶ = 64 total addresses  
    64 - 2 = 62 usable IPs
    

---

## 📐 Quick Subnetting Cheat Sheet

| CIDR | Subnet Mask | \# of Hosts |
| --- | --- | --- |
| /24 | 255.255.255.0 | 254 |
| /25 | 255.255.255.128 | 126 |
| /26 | 255.255.255.192 | 62 |
| /27 | 255.255.255.224 | 30 |
| /28 | 255.255.255.240 | 14 |
| /29 | 255.255.255.248 | 6 |
| /30 | 255.255.255.252 | 2 |

---

## 🧑‍🏫 Real-World Use Case

Let’s say you’re a network engineer for a small office:

* HR department needs 30 IPs
    
* IT needs 20 IPs
    
* Finance needs 10 IPs
    

You can subnet `192.168.1.0/24` into:

* `/27` for HR (30 usable)
    
* `/27` for IT (30 usable)
    
* `/28` for Finance (14 usable)
    

This avoids wasting a full `/24` subnet and keeps departments isolated!

---

## 🔚 Conclusion

Subnetting is a key skill for anyone working in networking, system administration, or cloud infrastructure. It allows you to design more efficient, secure, and scalable networks.

Yes, it may seem math-heavy at first, but with a little practice, it becomes second nature. Start by experimenting with small subnet examples and using online tools to verify your answers.