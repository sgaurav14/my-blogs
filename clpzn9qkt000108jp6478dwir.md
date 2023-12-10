---
title: "Understanding AWS S3 Buckets: A Beginner's Guide"
datePublished: Sun Dec 10 2023 15:33:26 GMT+0000 (Coordinated Universal Time)
cuid: clpzn9qkt000108jp6478dwir
slug: understanding-aws-s3-buckets-a-beginners-guide
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1702222241239/c9659362-7b96-4873-b1b7-5f934228e05e.png
tags: aws, aws-s3, s3-bucket, aws-s3-for-beginners, awswithtws-7daysofaws, 7daysofaws

---

Welcome to the world of cloud storage! If you're new to the concept, don't worry – we're here to guide you through the basics. In this comprehensive guide, we'll delve into AWS S3 (Simple Storage Service), Amazon's cloud storage solution. By the end of this journey, you'll have a solid understanding of what S3 is and how you can use it to securely store and manage your data in the cloud.

## **What is AWS S3?**

### **Understanding Cloud Storage**

Let's start with the basics. Cloud storage is like having a virtual hard drive on the internet. It allows you to store and retrieve your files from anywhere with an internet connection.

### **Introducing AWS S3**

AWS S3, or Simple Storage Service, is Amazon's cloud storage solution. Think of it as a vast digital warehouse where you can store any type of data – from documents and images to videos and application backups.

## **Getting Started with AWS S3**

### **Creating an AWS Account**

To use AWS S3, you'll need an AWS account. We'll walk you through the process of creating one, so you can access a range of AWS services, including S3.

### **Navigating the AWS Management Console**

Once you're in, we'll guide you through the AWS Management Console, your central hub for managing all your AWS resources. Find the S3 service and enter the world of cloud storage.

### **Creating Your First S3 Bucket**

A bucket in S3 is like a container for your files. Learn how to create your first bucket, choosing a unique name and configuring basic settings.

1. Search for "S3" in the AWS Management Console search bar and click on the Amazon S3 service in the results.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702218944494/848412fe-acd8-4b0b-940a-088f0bba3482.png align="center")
    
2. After clicking on 'S3' in the AWS Management Console search results, you'll access the S3 dashboard, which initially displays information in the Global region. Keep in mind that the buckets you create will be specific to the region you designate.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702219062514/f350d076-f338-4b1c-86e2-44a75cefecd2.png align="center")
    
    Click on the "Create Bucket".
    
3. Select the AWS region where you want your bucket to be located. This is the physical location of the servers storing your data. Choose a region that is geographically closest to your primary audience or where you expect the majority of your users to be.
    
    Choose a unique and meaningful name for your bucket. Bucket names must be globally unique across all of AWS, so you might need to get creative if your preferred name is already in use.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702219196565/d187613b-434c-4506-bbee-212934814006.png align="center")
    
4. When configuring ownership for other AWS accounts, if ACLs are required, enable the Object Ownership section. However, for a basic S3 bucket, where ACLs are not needed, proceed with ACLs disabled mode.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702219373534/da640663-03e4-45b9-99d6-2d3afc331532.png align="center")
    
5. If you intend to make your S3 bucket public, leave the 'Block Public Access settings for this bucket' option unchecked. However, if you prefer to restrict public access, you can check this option and specify individual permissions.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702219479783/b625366d-5a2d-485b-9158-9d43b15d6769.png align="center")
    
    After unchecking 'Block all public access,' a pop-up will appear, prompting you to confirm your intentional decision to make the bucket public. Note that AWS recommends blocking all public access for enhanced security.
    
6. Now, let's dive into a crucial feature: Bucket Versioning. Picture working on a document, making changes along the way. With versioning, S3 records every modification, creating multiple copies at different points in time. This acts as a safety net, preventing accidental deletions. If you ever make a mistake or need to revert to an earlier version, it's a simple process. Stay tuned as we delve deeper into this topic in our upcoming blogs. However, for now, we've chosen to disable the Bucket Versioning option.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702219781295/20052038-0019-4e6a-9aea-5273a5049935.png align="center")
    
    You have the option to add tags if desired.
    
7. In the 'Default Encryption' section, choose 'Server-side encryption with Amazon S3 managed keys (SSE-S3)' for data security. Additionally, in the 'Advanced settings,' consider enabling Object Lock for added protection. This feature ensures that objects in the bucket cannot be deleted unless explicitly allowed by the original owner, contingent on enabling bucket versioning.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702220190043/eed0d2c0-d3a3-4c64-8c61-81e16ed28c43.png align="center")
    
    Simply click on 'Create Bucket' to complete the setup.
    
8. Fantastic! Congratulations on successfully creating your first AWS S3 bucket!
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702220255051/b1b0aa36-524e-4ae1-a28e-410d2d4e2f8b.png align="center")
    
9. Let's dive into this bucket and upload some files/objects. Click on the bucket, then select the 'Upload' option. Choose any files you'd like to upload.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702220333650/d7f34254-3dfc-4706-bd43-773bac3d8645.png align="center")
    
    When you click on the "Upload" option, you'll be directed to the following screen:
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702220384961/7cf21825-36f1-4893-8bcd-6d31e072542e.png align="center")
    
    Click on "Add files" and choose the file you want to upload.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702220441541/eda47934-e858-4bbe-b2ce-4d34511be5b3.png align="center")
    
    After selecting your file, click on the "Upload" option to initiate the file upload process.
    
10. Great news! Your file has been successfully uploaded to your newly created S3 bucket. If you have more files to upload, repeat the process, or feel free to explore additional features of AWS S3. If you have any questions or tasks related to your S3 bucket or any other AWS services, feel free to ask. Congratulations on your successful upload!
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702220494690/8740fd15-2860-40a5-affe-fb2c47727f53.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702220517929/4070de8f-bc1f-4af1-9750-5677d0c80bda.png align="center")
    
11. Clicking on the uploaded object will lead you to the following screen:
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702220784654/ec7233d7-719c-430c-b809-cc210422ebdb.png align="center")
    
    Click on the "Open" option located in the top right-hand corner.
    
12. Upon clicking "Open," you will be redirected to your object in a new tab.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702220903360/9cc84bb5-5dff-4946-a7aa-2f07b417be86.png align="center")
    

Excellent! You've completed the basic operations on AWS S3 buckets. Now, let's move on to learning how to delete S3 buckets.

1. Navigate to your S3 bucket dashboard, choose your bucket, and select the "Delete" option to proceed with the deletion.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702221041823/7ac5fc44-dcf5-4fea-a1b1-b2362b078e15.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702221054858/1c517aec-4ed4-4f08-bf28-f01ae14f6f74.png align="center")
    
    The error indicates that the bucket must be empty before deletion. To proceed with the deletion, ensure that the bucket is empty by removing all objects from it. Once the bucket is empty, you should be able to delete it without encountering this error.
    
2. Return to the S3 dashboard, choose your S3 bucket, and click on "Empty" to remove all objects from the bucket.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702221182309/a3f8076e-6a98-4408-8f02-c2a41d7a51ac.png align="center")
    
    Upon clicking, the bucket will be emptied, and all objects within it will be removed.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702221226153/af72853e-9b34-4525-902f-2ad5323fb034.png align="center")
    
    The bucket is empty now. We can delete this bucket now. Click on Exit and go for deletion of this bucket.
    
3. Choose the bucket and click on "Delete" to proceed with the deletion process.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702221296983/ea3397e0-c556-4a05-b338-17436be8d015.png align="center")
    
    Enter your bucket name in the provided field and click on "Delete Bucket" to confirm and complete the deletion process.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702221332978/e87e03b9-0bb5-4b9a-9eb0-ee1eb514cee1.png align="center")
    
4. Fantastic! Your bucket has been successfully deleted.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702221370881/a0541cb9-76c4-4105-a79a-e021da888855.png align="center")
    

# **Conclusion**

Congratulations! You've successfully navigated through the fundamental operations of AWS S3, from creating your first bucket to uploading and exploring objects. Let's recap the key steps you've taken:

1. **Bucket Creation:**
    
    * Created your first AWS S3 bucket, providing it a unique name and specifying the region for storage.
        
2. **File Upload:**
    
    * Uploaded files into your S3 bucket by utilizing the simple and intuitive upload process.
        
3. **Object Exploration:**
    
    * Navigated through your bucket to explore and view the objects (files) you uploaded.
        
4. **Deletion Process:**
    
    * Learned how to empty a bucket and successfully delete it, understanding the importance of an empty bucket for deletion.
        

These foundational steps empower you to manage and organize your data effectively in the AWS cloud. As you delve deeper into AWS S3, you can explore advanced features, such as versioning, encryption, and access control, to tailor your storage solutions to specific needs.

Remember, AWS provides a robust and scalable environment, and this guide serves as a starting point for your cloud journey. If you have more questions or if there's anything specific you'd like to explore further, feel free to reach out. Happy exploring and utilizing the capabilities of AWS S3!