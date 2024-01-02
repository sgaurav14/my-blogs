---
title: "A Beginner's Guide to AWS EC2 Snapshots and Restoration"
datePublished: Tue Jan 02 2024 15:21:49 GMT+0000 (Coordinated Universal Time)
cuid: clqwhzemc000008l4bwog934t
slug: a-beginners-guide-to-aws-ec2-snapshots-and-restoration
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1704208850669/5a670ac5-bee3-45e2-941c-0cfc3545e069.png
tags: ec2, aws, ec2-snapshot, ec2-snapshot-restoration

---

Amazon Elastic Compute Cloud (EC2) is a widely used cloud computing service that allows users to run virtual servers in the cloud. One of the key features of EC2 is the ability to create snapshots of your virtual machine's (instance) storage volumes. These snapshots serve as a point-in-time backup of your data and can be invaluable for data recovery, system updates, or creating duplicate instances. In this guide, we'll walk you through the process of creating EC2 snapshots and restoring instances from those snapshots.

### **Understanding AWS EC2 Snapshots:**

An EC2 snapshot is a copy of your Amazon EBS (Elastic Block Store) volume at a specific point in time. EBS volumes are the block-level storage devices attached to your EC2 instances. Snapshots capture the entire volume, including the operating system, applications, and data. Here's how you can create an EC2 snapshot:

1. **Access AWS Management Console:** Log in to your AWS Management Console and navigate to the EC2 Dashboard.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704205790148/05ef5757-2a22-4338-b0cb-6858f673c7b0.png align="center")
    
2. **Select your Instance:** Identify the EC2 instance for which you want to create a snapshot and select it.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704205850816/9374df6f-6fce-4fa6-8d41-02c57cf1bb2f.png align="center")
    
3. **Create Snapshot:** In the "Description" tab, locate the "Block devices" section. Identify the root volume (usually "/dev/xvda1") and click on the volume ID. From the volume details page, click the "Actions" button, and select "Create Snapshot."
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704205883359/44d46a90-3482-4ef8-b71c-535edc02b6a2.png align="center")
    
    Select the volume under the Volume ID section and click on it. It will redirect to the Volumes options.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704205906903/67618e74-5f07-4f66-8fd3-7666e2965964.png align="center")
    
4. **Snapshot Configuration:** Provide a meaningful name and description for the snapshot. Click "Create Snapshot" to initiate the process.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704205938508/25a82712-322e-42cd-94bf-13ada22d615f.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704206751562/55293a01-8db7-4141-a889-5a444722131e.png align="center")
    

### **Monitoring Snapshot Progress:**

Once the snapshot creation process begins, you can monitor its progress in the AWS Management Console. Snapshots are incremental, meaning only the data that has changed since the last snapshot is copied. This helps in optimizing storage usage and reducing costs.

### **Restoring an EC2 Instance from a Snapshot:**

Now that you have a snapshot, you can use it to restore an EC2 instance or create a new one based on that snapshot. Follow these steps:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704206120085/5b8774a2-37d6-4e16-8141-958ca3b61d1a.png align="center")

1. **Navigate to Snapshots:** In the EC2 Dashboard, go to the "Snapshots" section, and select the snapshot you want to use.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704206869325/73509e53-0268-4b76-9251-007b7740b47b.png align="center")
    
    1. **Create Volume from Snapshot:** Click on "Actions" and choose "Create Volume." This will create a new EBS volume based on the selected snapshot.
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704206891866/7d52fb97-a4bc-472a-997a-3c371d06d9f7.png align="center")
        
        Click on 'Create volume from snapshot'.
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704206933925/91cf59cb-df87-4ae4-a32e-5dfb0d283b5e.png align="center")
        
        Here the most important option is the Availability Zone. We have to select the same AZ where our EC2 instance is configured.
        
        In our case, it is us-east-2b.
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704206954616/2ca126d5-7b95-4de7-bb2a-2a6eb9171f10.png align="center")
        
        Click on 'Create volume'.
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704206973236/f92679d3-d727-483b-a0a5-2bccdebbaedc.png align="center")
        
2. **Attach Volume to an Instance:** Go to the "Volumes" section in the EC2 Dashboard.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704207012477/e279afe4-9d6e-4c1e-b6eb-68ae8fee5a8c.png align="center")
    
    Now we have to detach the old root volume which we are imaging that it is corrupted and we will bring that server up with the new snapshot volume.
    
    Before detaching the volume, the EC2 instance must be in stopped state.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704207032093/fa90df2a-ad89-4c20-8a8a-c8e66599f812.png align="center")
    
    Select the volume, Click on 'Actions' and then click on 'Detach volume'.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704207056292/9871a0a6-3e28-48a2-af5e-e24b9c91c6b5.png align="center")
    
    Click on 'Detach'.
    
    We can see that both volumes are in the Available state.  
    I have given the name to both the volumes, 'old-vol 'the primary volume, and the new volume which we created from the snapshot as 'new-vol'.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704207169031/c2e6aa1c-7ecf-497c-b49f-795edc35c9bf.png align="center")
    
    Now we have to attach this new volume to our EC2 instance.
    
    Select the new volume and click on 'Actions', then click on 'Attach volume'.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704207199988/958317e8-3ac9-4362-9475-79f164118d4d.png align="center")
    
    Give the correct device name in the 'Device name' section if you give an incorrect name, your EC2 will not come up.
    
    In our case the root volume name of our EC2 instance is /dev/xvda so we have given that.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704207220656/f1e2e6cb-5ba5-4fac-8c53-508e6b0da920.png align="center")
    
    Now, our new-vol is attached to the EC2 instance.
    
3. **Start the Instance:** If the instance is not running, start it. Once the instance is running, the attached volume will be available.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704207398821/de20c6b0-b038-43b7-a32a-8ef31c51bb66.png align="center")
    
4. **Verify the Restoration:** Log in to the instance and ensure that the data and configurations are as expected. You've successfully restored an EC2 instance from a snapshot!
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704207441574/aff54300-ac84-41e0-b7fb-24a892b9cbc2.png align="center")
    
    Hurray!!, We can see that the EC2 instance came up with the volume that we created from the snapshot with all the data.
    

### **Tips and Best Practices:**

* **Regularly Create Snapshots:** Schedule periodic snapshots to ensure you have recent backups in case of data loss or system failure.
    
* **Snapshot Retention:** Establish a snapshot retention policy to manage and clean up older snapshots to control costs.
    
* **Test Restorations:** Periodically test the restoration process to verify that your snapshots are functional and your recovery strategy is effective.
    

### Conclusion:

AWS EC2 snapshots are a powerful tool for safeguarding your data and ensuring business continuity in the cloud. By understanding how to create snapshots and restore instances from them, you empower yourself with a robust backup and recovery strategy in your AWS environment. Regularly practicing these processes will help you navigate potential challenges with confidence and efficiency.