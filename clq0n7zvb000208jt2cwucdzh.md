---
title: "Hosting a Static Website on Amazon S3"
datePublished: Mon Dec 11 2023 08:19:50 GMT+0000 (Coordinated Universal Time)
cuid: clq0n7zvb000208jt2cwucdzh
slug: hosting-a-static-website-on-amazon-s3
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1702282697473/fb5580ba-7947-4790-8bc1-93b3f0862ce2.png
tags: aws, s3, s3-bucket, static-website-on-s3

---

In today's digital era, having a strong online presence is essential for individuals and businesses alike. Whether you're showcasing a portfolio, running a blog, or hosting a simple informational site, a reliable and cost-effective hosting solution is crucial. Amazon Simple Storage Service (S3) provides an excellent platform for hosting static websites, combining scalability, security, and ease of use.

## **Why Choose Amazon S3 for Hosting?**

### **1\. Cost-Effective**

Amazon S3 offers a pay-as-you-go pricing model, making it cost-effective for hosting static websites. You pay only for the storage you use and the data transfer associated with your site.

### **2\. Scalability**

S3 can easily scale to handle varying levels of traffic. Whether your website is just starting or experiences sudden spikes in traffic, S3 can seamlessly accommodate the demand.

### **3\. High Availability and Durability**

Amazon S3 provides a highly available and durable infrastructure. Your static website will benefit from Amazon's robust data centers and global content delivery network (CDN), ensuring reliable access to your content.

### **4\. Simple Configuration**

Setting up a static website on Amazon S3 is straightforward. With a few simple steps, you can have your site up and running in no time.

## **Step-by-Step Guide: Hosting a Static Website on S3**

### **1\. Create an Amazon S3 Bucket**

1. **Sign in to the AWS Management Console:** Navigate to the AWS Management Console and sign in to your AWS account.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702278488683/edbc8091-5340-4d66-ba8b-58a77081feb8.png align="center")
    
2. **Access S3:** Open the S3 console.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702278513378/3c7a8fa3-7459-4064-a5de-ca22e0805627.png align="center")
    
3. **Create a Bucket:** Click the "Create bucket" button and follow the prompts to set up a new bucket. Choose a unique and meaningful name for your bucket.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702279816737/59cad94c-1f94-4765-bb70-01ec9428cd64.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702279818803/855823ca-9d2c-4071-93d9-bd3de2933989.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702279835054/9ffe5c00-50b9-452c-bba2-65b48b43da24.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702279846851/1a74386d-763c-435b-af32-8e8d8b250f11.png align="center")
    
    Now my bucket is created.
    

### **2\. Configure the Bucket for Website Hosting**

1. **Select the Bucket:** In the S3 console, select the bucket you just created.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702279879780/d823d7cb-5a19-4fe2-95c8-6ef8a314d49d.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702279890915/f4976456-b18c-454a-8f86-d451ac7b46fe.png align="center")
    
2. **Enable Static Website Hosting:** Navigate to the "Properties" tab and select "Static website hosting." Enable the feature and specify the index document (e.g., `index.html`) and optional error document.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702279920969/5c032a21-e9ef-47ff-b0cb-500dfb370b6b.png align="center")
    
    Scroll down and select "Static website hosting".
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702279944044/77d534b1-ebee-46f7-96e6-fa88d488b311.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702279966283/64e73762-219b-42db-b08a-5e4a5be22c19.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702280044352/6bef6b56-9e0c-4822-8a1d-8957f58b057f.png align="center")
    
    We have given redirection rules as if my website ends with www.example.com/home, it will redirect to our index.html.
    

### **3\. Upload Your Website Content**

1. **Upload Files:** Go to the "Overview" tab, click "Upload," and upload your static website files, including HTML, CSS, JavaScript, and any other assets.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702280068771/859bd626-ba6c-4fae-8bdc-0032de85649f.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702280097919/28c49a98-b40c-492e-a7ee-31555d1a844d.png align="center")
    
    We created the above index.html and error.html with the help of ChatGPT. If you are a developer, you can create your file.
    
2. **Set Permissions:** Make sure your files are configured with the correct permissions. Select the files, click "Actions," and choose "Make public" to grant public read access. Our bucket is already public. But we need to configure Bucket Policy to make our website reachable for everyone.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702280367422/a4f73ad9-014c-4d08-901d-ed69c7d0643a.png align="center")
    
    Edit the Bucket policy:
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702280308473/45e3c8e6-3de3-4991-8f5f-04d9f4e95ec3.png align="center")
    

### **4\. Obtain the Website Endpoint**

1. **Access the Endpoint:** Once your files are uploaded, go to the "Static website hosting" tab to find the endpoint URL. This is the address where your website will be accessible.
    
    Go to your bucket -&gt; Select your bucket -&gt; Click on Properties -&gt; Scroll down to Static website hosting option.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702280422298/2a7e13cb-b488-424a-8124-7e88776951f7.png align="center")
    

### **5\. Test Your Website**

Open a web browser and enter the endpoint URL. If everything is configured correctly, you should see your static website.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702280708043/6aa10188-1d90-466c-b347-6936b09b0bb2.png align="center")

If we are doing something wrong, we are getting our error page as well.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702280747150/d5d2fb9d-d4f5-4a21-9abb-7dddae0ee5f9.png align="center")

Click on "home".

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702280766203/56fcd885-3cb2-4b72-a1e0-244f619df5d6.png align="center")

Hurray! Our first static website hosted on AWS S3 is now up and running. 🚀

## **Conclusion**

Amazon S3 offers a simple, scalable, and cost-effective solution for hosting static websites. By following the steps outlined in this guide, you can have your website up and running on S3 in no time. Take advantage of the flexibility and reliability of Amazon S3 to ensure a smooth online experience for your visitors.

Start hosting your static website on Amazon S3 today and enjoy the benefits of a reliable and scalable hosting solution.