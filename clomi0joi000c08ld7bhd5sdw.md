---
title: "Mastering the Basics: A Non-Technical Guide to Essential Linux Commands"
datePublished: Mon Nov 06 2023 06:05:36 GMT+0000 (Coordinated Universal Time)
cuid: clomi0joi000c08ld7bhd5sdw
slug: mastering-the-basics-a-non-technical-guide-to-essential-linux-commands
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1699178401922/4dc9f924-ccf7-41aa-8330-f65e87b57532.jpeg
tags: linux, linuxbasiccommands

---

**Introduction**:

In today's digital age, whether you're a student, professional, or simply someone who loves exploring the world of technology, a basic understanding of Linux commands can be an empowering skill. Don't be discouraged by the jargon or the tech-savvy image of Linux—these fundamental commands are accessible to everyone, regardless of their technical background.

Think of Linux commands as the keys to a powerful toolbox that can help you navigate, create, and manage files and directories on your computer. They provide you with a bridge to the inner workings of your operating system, allowing you to perform tasks, organize your digital life, and troubleshoot issues.

In this guide, we'll walk you through these essential Linux commands in a way that's friendly, approachable, and devoid of technical mystique. Whether you're a student looking to improve your computer skills, a professional seeking to streamline your work, or just someone curious about what makes Linux tick, this is the place to start.

**1\. Navigating the File System**:

* **Present Working Directory (**`pwd`): This is like checking your current location on a map. It tells you where you are in your computer's file system.
    
    ```bash
    [root@sid-vm ~]# pwd
    /root
    [root@sid-vm ~]#
    ```
    
* **List Files and Directories (**`ls`): Think of this as looking inside a folder to see what's in it. It shows you a list of all the files and folders in your current location.
    
    ```bash
    [root@sid-vm /]# ls
    bin  boot  data  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
    [root@sid-vm /]#
    ```
    
    If you want to long list the files and directories to get the complete details, use "ls -l" command.
    
    ```bash
    [root@sid-vm /]# ls -l
    total 32
    lrwxrwxrwx.   1 root root    7 Jul 18 11:11 bin -> usr/bin
    dr-xr-xr-x.   4 root root 4096 Jul 19 04:32 boot
    drwxr-xr-x.   2 root root    6 Oct 29 04:55 data
    drwxr-xr-x.  21 root root 3320 Oct 31 07:06 dev
    drwxr-xr-x. 109 root root 8192 Oct 31 07:06 etc
    drwxr-xr-x.   2 root root    6 Apr 10  2018 home
    lrwxrwxrwx.   1 root root    7 Jul 18 11:11 lib -> usr/lib
    lrwxrwxrwx.   1 root root    9 Jul 18 11:11 lib64 -> usr/lib64
    drwxr-xr-x.   2 root root    6 Apr 10  2018 media
    drwxr-xr-x.   2 root root    6 Apr 10  2018 mnt
    drwxr-xr-x.   3 root root   16 Jul 18 11:23 opt
    dr-xr-xr-x. 198 root root    0 Oct 31 07:06 proc
    dr-xr-x---.   5 root root 4096 Jul 19 04:30 root
    drwxr-xr-x.  34 root root 1140 Oct 31 07:06 run
    lrwxrwxrwx.   1 root root    8 Jul 18 11:11 sbin -> usr/sbin
    drwxr-xr-x.   2 root root    6 Apr 10  2018 srv
    dr-xr-xr-x.  13 root root    0 Oct 31 07:06 sys
    drwxrwxrwt.   8 root root 4096 Nov  5 04:27 tmp
    drwxr-xr-x.  13 root root 4096 Jul 18 11:11 usr
    drwxr-xr-x.  20 root root 4096 Jul 18 11:39 var
    [root@sid-vm /]#
    ```
    
* **Change Directory (**`cd`): This is like moving from one room to another. It helps you go into different folders or locations.
    
    ```bash
    [root@sid-vm /]# pwd
    /
    [root@sid-vm /]#
    [root@sid-vm /]# ls
    bin  boot  data  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
    [root@sid-vm /]#
    [root@sid-vm /]#
    [root@sid-vm /]# cd tmp/
    [root@sid-vm tmp]#
    [root@sid-vm tmp]# pwd
    /tmp
    [root@sid-vm tmp]#
    ```
    

**2\. Creating and Managing Directories**:

* **Make a New Directory (**`mkdir`): Creating a directory is like making a new folder to store your stuff. You give it a name, and it appears in your current location.
    
    ```bash
    [root@sid-vm tmp]# ls
    [root@sid-vm tmp]#
    [root@sid-vm tmp]# mkdir testdir
    [root@sid-vm tmp]#
    [root@sid-vm tmp]# ls -l
    total 0
    drwxr-xr-x. 2 root root 6 Nov  5 04:30 testdir
    [root@sid-vm tmp]#
    ```
    
* **Create Nested Directories (**`mkdir -p`): Nested directories are like having folders inside folders. This command helps you create folders within other folders.
    
    ```bash
    [root@sid-vm tmp]# mkdir -p dev/test/deploy
    [root@sid-vm tmp]#
    [root@sid-vm tmp]# ls -l
    total 0
    drwxr-xr-x. 3 root root 18 Nov  5 04:31 dev
    [root@sid-vm tmp]# cd dev/
    [root@sid-vm dev]# ls -l
    total 0
    drwxr-xr-x. 3 root root 20 Nov  5 04:31 test
    [root@sid-vm dev]#
    [root@sid-vm dev]# cd test/
    [root@sid-vm test]# ls -l
    total 0
    drwxr-xr-x. 2 root root 6 Nov  5 04:31 deploy
    [root@sid-vm test]#
    ```
    

**3\. Creating and Viewing Files**:

* **Create an Empty File (**`touch`): Think of this as creating a blank piece of paper. You can give it a name, and it's ready for you to write on.
    
    ```bash
    [root@sid-vm tmp]# touch test.txt
    [root@sid-vm tmp]#
    [root@sid-vm tmp]# ls -l
    total 0
    -rw-r--r--. 1 root root 0 Nov  5 04:33 test.txt
    [root@sid-vm tmp]#
    ```
    
* **Display File Contents (**`cat`): This command lets you open and read what's written on a piece of paper (a file). It displays the text inside the file.
    
    ```bash
    [root@sid-vm tmp]# cat test.txt
    Hello, You are reading Siddhartha technical blogs!!
    [root@sid-vm tmp]#
    ```
    
* **Add Content to a File (**`echo`): Imagine typing or writing on that piece of paper. `echo` allows you to add text to a file.
    
    ```bash
    [root@sid-vm tmp]# echo "I am ready to learn Linux. Are you ready?" > abc.txt
    [root@sid-vm tmp]#
    [root@sid-vm tmp]# cat abc.txt
    I am ready to learn Linux. Are you ready?
    [root@sid-vm tmp]#
    ```
    

**4\. Managing Permissions**:

We will cover this topic in a separate blog, but for now, we will understand what command we can use to manage permissions in Linux.

* **Modify File and Directory Permissions (**`chmod`): Permissions are like locks on a door. `chmod` helps you control who can open the door (access files and folders).
    
    ```bash
    [root@sid-vm tmp]# ls -l
    total 4
    -rw-r--r--. 1 root root 52 Nov  5 04:34 test.txt
    [root@sid-vm tmp]#
    [root@sid-vm tmp]#
    [root@sid-vm tmp]# chmod 775 test.txt
    [root@sid-vm tmp]#
    [root@sid-vm tmp]# ls -l
    total 4
    -rwxrwxr-x. 1 root root 52 Nov  5 04:34 test.txt
    [root@sid-vm tmp]#
    ```
    
* **Change File and Directory Ownership (**`chown`): It's like changing the owner of a house. You can transfer ownership of files and folders to someone else.
    
    ```bash
    [root@sid-vm tmp]# ls -l
    total 4
    -rwxrwxr-x. 1 root root 52 Nov  5 04:34 test.txt
    [root@sid-vm tmp]#
    [root@sid-vm tmp]# chown labuser test.txt
    [root@sid-vm tmp]#
    [root@sid-vm tmp]# ls -l
    total 4
    -rwxrwxr-x. 1 labuser root 52 Nov  5 04:34 test.txt
    [root@sid-vm tmp]#
    [root@sid-vm tmp]# ls -l
    total 4
    -rwxrwxr-x. 1 labuser root 52 Nov  5 04:34 test.txt
    [root@sid-vm tmp]#
    [root@sid-vm tmp]# chgrp labuser test.txt
    [root@sid-vm tmp]#
    [root@sid-vm tmp]# ls -l
    total 4
    -rwxrwxr-x. 1 labuser labuser 52 Nov  5 04:34 test.txt
    [root@sid-vm tmp]#
    ```
    

**5\. Deleting Files and Directories**:

* **Remove Files and Directories (**`rm`): This is like throwing away a piece of paper or an empty folder. It deletes files and folders.
    
    ```bash
    [root@sid-vm tmp]# ls
    test.txt
    [root@sid-vm tmp]#
    [root@sid-vm tmp]# rm test.txt
    rm: remove regular file ‘test.txt’? yes
    [root@sid-vm tmp]#
    ```
    
* **Delete Empty Directories (**`rmdir`): Use this when you want to throw away an empty folder.
    
    ```bash
    [root@sid-vm tmp]# ls -l
    total 0
    drwxr-xr-x. 2 root root 6 Nov  5 04:42 testdir
    [root@sid-vm tmp]#
    [root@sid-vm tmp]# cd testdir/
    [root@sid-vm testdir]# ls
    [root@sid-vm testdir]# cd .
    [root@sid-vm testdir]# cd ..
    [root@sid-vm tmp]#
    [root@sid-vm tmp]# rm testdir/
    rm: cannot remove ‘testdir/’: Is a directory
    [root@sid-vm tmp]# rm -rf testdir/
    [root@sid-vm tmp]#
    ```
    
* **Forcefully Remove Files and Directories (**`rm -rf`): Be careful with this! It's like using a super strong delete button. It can remove everything, so use it with caution.
    

**6\. Viewing and Editing Files**:

* **Display File Contents (**`cat`): This is like reading the text on a piece of paper.
    
* **Basic Text Editing (**`nano` or `vim`): Think of this as being able to write and edit on that piece of paper. `nano` or `vim` are like simple word processors.
    
    ```bash
    [root@sid-vm tmp]# vim hello.txt
    Hello friends, Linux Seekh lo...
    ~
    ~
    ~
    ~
    ~
    ~
    
    :wq
    [root@sid-vm tmp]# cat hello.txt
    Hello friends, Linux Seekh lo...
    [root@sid-vm tmp]#
    ```
    

**7\. Viewing the Beginning and End of Files**:

* **Display the Start of a File (**`head`): It's like reading the first few lines of a book.
    
    (by default it will show top 10 lines)
    
    ```bash
    [root@sid-vm tmp]# head exam-poem.txt
    Today we got the first pop quiz
    I could actually do!
    Usually when we take a test
    I haven’t got a clue.
    
    The first question was easy:
    Find a booger in your nose.
    Then wipe it on the paper
    instead of on your clothes.
    
    [root@sid-vm tmp]#
    ```
    
* **Show the End of a File (**`tail`): This is like reading the last few lines of a book.
    
    (by default it will show bottom 10 lines)
    
    ```bash
    [root@sid-vm tmp]# tail exam-poem.txt
    
    The first question was easy:
    Find a booger in your nose.
    Then wipe it on the paper
    instead of on your clothes.
    
    I had to list ten chocolate bars
    and nine video games.
    Then I had to come up
    with a dozen crazy claims.
    [root@sid-vm tmp]#
    ```
    

**8\. Comparing Two Files**:

* **Find Differences Between Two Files (**`diff`): Imagine having two different drafts of an essay. `diff` helps you identify the changes between them.
    
    ```bash
    [root@sid-vm tmp]# cat fruits.txt
    Apple
    Mango
    Banana
    Cherry
    Kiwi
    Orange
    Guava
    [root@sid-vm tmp]#
    [root@sid-vm tmp]# cat colors.txt
    Red
    Pink
    White
    Black
    Blue
    Orange
    Purple
    Grey
    [root@sid-vm tmp]#
    [root@sid-vm tmp]# diff fruits.txt colors.txt
    1,5c1,5
    < Apple
    < Mango
    < Banana
    < Cherry
    < Kiwi
    ---
    > Red
    > Pink
    > White
    > Black
    > Blue
    7c7,8
    < Guava
    ---
    > Purple
    > Grey
    [root@sid-vm tmp]#
       
    ```
    

Remember, you don't need to be a tech expert to use these commands. They are like simple tools to help you manage and work with files and folders on your computer. Just practice and take your time, and you'll become more comfortable with them.