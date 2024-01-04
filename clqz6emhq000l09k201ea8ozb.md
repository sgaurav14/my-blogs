---
title: "Understanding AWS VPC Peering: A Beginner's Guide"
datePublished: Thu Jan 04 2024 12:21:02 GMT+0000 (Coordinated Universal Time)
cuid: clqz6emhq000l09k201ea8ozb
slug: understanding-aws-vpc-peering-a-beginners-guide
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1704370824676/44716839-dc49-4741-b518-1667e36efba7.png
tags: aws, networking, vpc, vpc-peering, vpc-basics

---

Amazon Web Services (AWS) offers a robust networking service called Virtual Private Cloud (VPC) that allows users to create a logically isolated section of the AWS Cloud where they can launch resources. VPC Peering is a powerful feature within AWS that enables seamless communication between VPCs, creating a virtual network that spans multiple AWS accounts.

## What is AWS VPC Peering?

VPC Peering is a networking connection between two VPCs that allows them to communicate with each other as if they were in the same network. This connection is established by using private IP addresses, which means that the traffic between the VPCs stays within the AWS network and doesn't traverse the public internet.

## Key Concepts

### Sender and Receiver VPCs

In a VPC Peering connection, there are two VPCs involved: the sender (initiating the peering request) and the receiver (accepting the peering request). Both VPCs can belong to the same AWS account or different AWS accounts.

### CIDR Blocks

CIDR (Classless Inter-Domain Routing) blocks are used to define the IP address ranges of the VPCs involved in the peering connection. It's crucial to ensure that the CIDR blocks do not overlap, as overlapping CIDR blocks would lead to routing conflicts.

## Lab on VPC Peering

In the below lab, we are going to perform below operations -

1. Create one VPC in Mumbai and one in Ohio. These two are isolated from each other as there are two different VPC.
    
2. As a part of VPC creation, create subnets, an Internet gateway and attach this internet gateway to the new VPC, create a custom route table and associate your subnets, and add your routes for the internet.
    
    We are picking below values for our lab -
    
    * Mumbai VPC CIDR: 10.0.0.0/16
        
        Mumbai Subnets: mum-cust-subnet1 - 10.0.1.0/24
        
        mum-cust-subnet2 - 10.0.2.0/24
        
    * Ohio VPC CIDR: 172.16.0.0/16
        
        Ohio Subnets: ohio-cust-subnet1 - 172.16.1.0/24
        
    
    Take reference for creating a VPC from the blog - [https://sgaurav.hashnode.dev/understanding-aws-vpc-a-beginners-guide](https://sgaurav.hashnode.dev/understanding-aws-vpc-a-beginners-guide)
    
3. Create one EC2 instance on all the subnets, it means two in Mumbai VPC and one in Ohio VPC.
    
4. Once instances are ready, will first ping the EC2 instances in the same subnet and then ping them in two different VPCs.
    
    * Below is the pictorial diagram of our lab -
        

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704365347014/d9ec2e92-9259-48a7-ad0a-274841435316.png align="center")

Below are the above lab screenshots:

### **Task 1: Mumbai VPC Creation :**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704366343710/5652480e-c29e-40c9-8bf9-9304dae3f52f.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704366405538/a0c3ccf2-79d9-4e08-bfe9-88eaff23ba24.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704366425722/72e533d7-0f34-42a5-a4d9-377445bc5277.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704366441528/bb34e2e6-442d-43be-ab9f-b393c91e484a.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704366487402/5d900de3-385d-4bc3-bb37-4476bdffb63b.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704366550880/1278511a-7b28-4a0a-8d81-8aafef4652fd.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704366569283/2b727032-b54a-462f-82a7-0205fd21f2d9.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704366582630/0eedb19d-c562-4cc0-ad1c-3c57fbceb60e.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704366620208/e460e66a-5307-4990-803e-2301ab646fcc.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704366634108/df0fb2e5-84a7-482b-96b3-51879da49d98.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704366649299/2f5471ad-d890-432f-a81b-88e5b4394572.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704366666656/2303f499-4df8-4e99-95d3-978bc4887fd0.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704366687486/47585821-aa10-4a3d-bade-01ad8c78ed38.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704366701949/adf102d3-960d-4dba-99d2-1826de55929e.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704366718144/0cb8fa30-3f1e-4c50-a992-36c3370e94b7.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704366733791/b12a9f60-00fb-4b7a-9f85-9472f1436c5d.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704366748189/0367a54e-b793-4e92-b92f-8bd76b6f9cc3.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704366763497/18b4bd88-e41c-4411-87dd-37e3cda88230.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704366781904/23c43ab2-dec3-4eda-876f-e3deabd0b8ae.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704366801976/ac2dbfeb-e309-4bfc-bb2a-5181834496db.png align="center")

**After this, launch one EC2 instance in both the subnets.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704367190823/1112cab5-8fad-4e9a-81d7-52001886c9c4.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704367206029/6d435639-866c-4e96-af4a-ef2461362c01.png align="center")

Let's ping both the EC2 instances from each other -

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704367272779/1f03c795-4946-43ec-b3cf-76327a795739.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704367317098/ff386324-8f9f-4d24-a3d1-3857a08f888b.png align="center")

Hurray, our first test case is passed, bot the EC2 instances are reachable to each other.

Similarly, create one VPC in the Ohio region and one subnet and all other stuff required for VPC.

### **Task 2 :Ohio VPC Creation :**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704367485244/edc4dbf0-a092-4294-8135-a6fc7605dfe0.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704367562608/303559b4-ecc3-47c0-b703-e4376e2187b3.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704367578834/10c1cbc2-7a68-4502-abda-ddc749ed5892.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704367604060/cfaf57ea-cca9-4f1d-a97a-b7dde675e148.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704367643638/fcf4fe40-97cb-4014-ae0b-dcfb3d112ec1.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704367661223/f6143573-606f-4388-8670-2d61d65df08d.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704367683947/4632be70-1820-49a9-ad32-a793522fc631.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704367701741/4b2eedc9-affe-43d2-95c6-997bf438b0ad.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704367712891/e31b15d9-6c5e-4427-84d1-374ef6a60ea9.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704367726873/c2e30606-45a0-4491-8bdd-541bb9d1b947.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704367740862/efaac852-54aa-42d9-990c-90afae90d23a.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704367755066/d12678a6-053b-4658-b9c0-c777cd5e5c15.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704367774740/821fb49d-0a96-49f7-81c7-91bf6e8c09bd.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704367790119/60ad51e7-5a1d-47f8-9216-25c4b384a3d3.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704367806837/16fef46e-7e5c-4cd0-809c-973605461cf3.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704367823226/f5505e6e-2af3-4c31-ae21-cec37f357025.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704367838399/f1ee555d-07b6-402f-998d-755ed6360411.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704367860187/e6f290a4-b889-4c7a-aa04-b7bef694f7e4.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704367872973/f3bb9a47-248c-4b3c-b0e6-b12cf527193e.png align="center")

Launch one EC2 instance in this custom Ohio VPC.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704368204401/ce332b6e-09c3-4cbd-a9b6-2900192686a0.png align="center")

## Task 3 : Ping the EC2 machines from both Custom VPC

Let's first ping the Mumbai EC2 instance from Ohio EC2.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704368426253/260bd490-e81c-466d-b5b7-1c0c63ec10a0.png align="center")

The Ohio EC2 instance is not pinging the Mumbai instance.

Let's check vice-versa.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704368477350/8b916bd5-9911-48d4-bc99-d0324ff4a565.png align="center")

The Mumbai EC2 instance is also not pinging the Ohio instance.

To make this work, we need to do VPC peering.

## Task 4 :How to Create VPC Peering

1. **Initiate Peering Request:**
    
    * In the AWS Management Console, navigate to the VPC dashboard.
        
    * Choose "Peering Connections" and click "Create Peering Connection."
        
    * Enter the account ID and VPC ID of the target VPC.
        
    
    Let's create this request from Mumbai to Ohio. We don't need to enter account ID as we are doing in the same account.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704368667678/a0ad57ca-0125-4993-8f18-d58b09f07363.png align="center")
    
    Click on 'Create peering connection'.
    
    Give the peering connection name and select your requestor VPC ID which is our custom Mumbai VPC ID.
    
    In option 'Select another VPC to peer with', select my account and select the another region as we created in Ohio region.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704368861700/44a93972-0cc2-4e04-b7ca-56e30c463764.png align="center")
    
    Click on 'Create peering connection'.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704368902432/abf9f233-4de7-4501-8ec6-9bdf78e03e5a.png align="center")
    
2. **Accept Peering Request:**
    
    * In the target VPC's AWS Management Console, navigate to "Peering Connections."
        
    * Select the peering connection request and accept it.
        
    
    Go to the accepter Peering region which is Ohio in our case.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704369382099/2e14a67c-3bb1-412d-b310-b144fa98dc0e.png align="center")
    
    We can see above that we have one request pending for acceptance.
    
    Select the request and click on 'Actions' -&gt; 'Accept request'.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704369585968/07aa7820-9f0f-4dd2-af27-be038be2cb5b.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704369598419/959e4c53-1e66-4b75-bd5f-ac0a189b6ca0.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704369616320/9d93c53d-a111-4a07-9fbe-b321e5c366ae.png align="center")
    
    Our peering connections are now done.
    
3. **Update Route Tables:**
    
    * Update the route tables in both VPCs to include routes for the CIDR block of the other VPC.
        
    
    Let's first update the Mumbai custom route table :
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704369708075/a5b8d5f0-97e9-47b3-ba8c-7e024ff871ef.png align="center")
    
    Click on 'Edit routes'.
    
    Give the Ohio custom VPC CIDR as destination and target is our newly created peering connection.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704369794939/d5d5dd3c-9076-4f75-919a-351f0f95415b.png align="center")
    
    Save the changes.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704369814941/49bf6c1c-ac34-4a81-9d90-ed48a4744e4a.png align="center")
    
    Now, do the same thing in Ohio custom route table.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704369861719/8fa3968d-95ba-4f45-ab01-fc3d34371985.png align="center")
    
    Click on 'Edit routes'.
    
    Give the Mumbai custom VPC CIDR as the destination and the target is our newly created peering connection.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704369900504/8c790096-7dd2-4286-a94d-014b12f3d444.png align="center")
    
    Save the changes.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704369964798/eb46bd88-1f7f-4f94-b4e8-e5f0a73670e8.png align="center")
    
    Finally, all of our tasks are now done. 😊😊
    
4. **Testing Connectivity:**
    
    * Once the peering connection is established, test connectivity between resources in the peered VPCs.
        
    
    First test ping from Ohio EC2 to Mumbai EC2 :
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704370101635/833cf873-c30a-4787-9350-3fb2452ed949.png align="center")
    
    Hurray!! our Ohio EC2 instance can reach the Mumbai EC2 and both are running in isolated VPC.can
    
    Now ping from Mumbai EC2 to Ohio EC2 :
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704370234856/7705338a-3fb4-46c3-9a6a-04f51e2f9442.png align="center")
    
    Vice-versa is also working fine. 😊😊
    

## Considerations and Best Practices

* **Transitive Peering:** VPC peering is not transitive, meaning if VPC A is peered with VPC B and VPC B is peered with VPC C, there is no direct connectivity between VPCs A and C. Separate peering connections are required.
    
* **Security Groups and Network ACLs:** Ensure that security groups and network ACLs are appropriately configured to allow the necessary traffic between peered VPCs.
    
* **Data Transfer Costs:** While data transfer between peered VPCs in the same region is free, there may be data transfer costs for inter-region peering.
    

## Conclusion

AWS VPC Peering simplifies network communication between VPCs, offering a secure and scalable solution for connecting resources across different accounts or within the same account. By understanding the basic concepts and following best practices, beginners can leverage VPC Peering to build flexible and efficient multi-VPC architectures on AWS.

Start exploring the capabilities of AWS VPC Peering to enhance the connectivity and collaboration of your cloud resources!