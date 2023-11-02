---
title: "Linux Architecture"
datePublished: Sat Oct 28 2023 18:04:12 GMT+0000 (Coordinated Universal Time)
cuid: cloacq08t000109jhdgng7nt0
slug: linux-architecture
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1698912180340/c73f217e-2121-45b5-93df-b27e10e2ff44.jpeg

---

We often come across the word "Linux" in our college or professional lives, but what exactly is Linux? Linux is an operating system, just like Windows or MacOS.

In most operating systems, we typically use a graphical user interface (GUI) to perform our tasks. However, Linux offers us the opportunity to work with a command-line terminal.

So, the next question that naturally arises is: what is this command-line terminal? To answer this, we must first grasp the architecture of Linux.

![Architecture of linux operating system - GeeksforGeeks](https://media.geeksforgeeks.org/wp-content/uploads/20200105215737/Untitled-Diagram-215-1.jpg align="left")

From the image above, it appears that the Linux system operates much like a solar system, where each component relies on the others. Let's delve into a better understanding of this diagram.

Typically, when we want to perform any task on a computer, we interact with the applications on the system. For example, if we need to calculate a sum, we communicate with a calculator application, which carries out the operation and provides us with the result. But what exactly happens in the background? Let's explore this.

When we open a terminal in Linux, which is an application that allows us to communicate with the system and perform various tasks, something interesting occurs. After opening the terminal, a shell appears, acting as the intermediary between the system and you. So, when we instruct the shell to add two numbers, the shell, in turn, communicates with the kernel. But what is this "Kernel"? The Kernel is the heart of the Linux operating system. It's the component responsible for interacting with hardware to execute tasks. Thus, when the shell requests the Kernel to perform an operation, such as adding two numbers, the Kernel interfaces with the hardware components like RAM, CPU, and hard disk to complete the task and deliver the output.

Here's the sequence when a query is made:

`Applications → Shell → Kernel → Hardware`

And when the system responds to your query:

`Hardware → Kernel → Shell → Applications`

This is how the Linux system operates. In the provided image, you can also see a utility section, which represents the packages in the Linux system. We will delve into these packages in more detail in the future.