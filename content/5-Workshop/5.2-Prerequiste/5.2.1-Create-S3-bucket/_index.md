---
title : "Create S3 Bucket"
date: 2025-09-09
weight : 1
chapter : false
pre : " <b> 5.2.1 </b> "
---

### Create S3 Bucket
#### What is an S3 Bucket?
An S3 bucket is a container for objects stored in Amazon S3. Think of it as a top-level folder containing your files. Each bucket has a globally unique name and exists in a specific AWS Region. Buckets serve as the fundamental organizing unit in S3 and provide a namespace for object storage.

#### Step-by-Step Instructions
1. Access S3 Service
    - Go to **AWS Management Console**
    - Find **S3** using the search bar or by navigating to the Storage service category
    - Select **S3**

2. Initiate Bucket Creation
    -   In the **S3** interface, select **Create bucket**

3. Configure Basic Bucket Settings

-   In the **Create bucket** interface:
    - **Bucket name**: Enter `workshop-demo-092025` (if the name is taken, add a number or use a custom name)
    - **AWS Region**: Select the region closest to your users or as required
    - **Object Ownership**: Select **ACLs disabled (recommended)**

![Create S3 Bucket](/images/5-Workshop/5.2-Prerequisite/5.2.1-Create-S3-bucket/1.png)
![Create S3 Bucket](/images/5-Workshop/5.2-Prerequisite/5.2.1-Create-S3-bucket/2.png)
![Create S3 Bucket](/images/5-Workshop/5.2-Prerequisite/5.2.1-Create-S3-bucket/3.png)
![Create S3 Bucket](/images/5-Workshop/5.2-Prerequisite/5.2.1-Create-S3-bucket/4.png)
![Create S3 Bucket](/images/5-Workshop/5.2-Prerequisite/5.2.1-Create-S3-bucket/5.png)

4. Verify Bucket Creation

Success! You have created an S3 bucket to store website source code. The bucket will be displayed under **General purpose buckets**

![Create S3 Bucket](/images/5-Workshop/5.2-Prerequisite/5.2.1-Create-S3-bucket/6.png)

#### What has been achieved
1.  Created a globally unique S3 bucket
2.  Configured default S3 settings
3.  Set up the foundation for static website hosting

#### Next Step
Next, we will upload the website source code to the S3 bucket and store it.