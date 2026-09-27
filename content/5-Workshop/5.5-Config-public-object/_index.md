---
title : "5. Configure Public Object"
date: 2025-09-09
weight : 5
chapter : false
pre : " <b> 5.5 </b> "
---

#### Step-by-Step Configuration

1. Access Bucket Permissions
  - In your S3 bucket interface, select the **Permissions** tab

![Permissions](/images/5-Workshop/5.5-Config-public-object/1.png)

2. Find Access Control List Settings

Scroll down to find the **Access control list (ACL)** section
  - You will see **Bucket owner enforced** is currently selected
  - This means ACLs are disabled and the bucket owner controls all objects

![Access control list](/images/5-Workshop/5.5-Config-public-object/2.png)

3. Enable ACL for Object Level Control
  - Select **Edit** in the Object Ownership section, then configure:
    - Object ownership: Select **ACLS enabled**
    - Acknowledgment: Check **I acknowledge that ACLs will be restored**
    - **Object ownership setting**: Select **Bucket owner preferred**
    - Select **Save changes**

![Object ownership](/images/5-Workshop/5.5-Config-public-object/3.png)

4. Verify ACL Configuration

After saving, you will see ACLs enabled in the Object Ownership section.
- Updated Configuration: Your bucket now supports ACLs, allowing you to set object-level permissions including public access.

![Object ownership](/images/5-Workshop/5.5-Config-public-object/4.png)

5. Make Objects Public Using ACL

Navigate back to the Objects tab of the bucket:
  - Select the objects or folders you want to make public
  - Select Actions from the toolbar
  - Select Make public using ACL

![Object ownership](/images/5-Workshop/5.5-Config-public-object/5.png)

6.  Confirm Public Access

On the Make public confirmation page:

  - Review the objects that will be made public
  - Understand that these objects will be accessible by anyone
  - Select Make public to confirm

**Final Warning**: When you click “Make public”, these objects will immediately be accessible by anyone on the internet who knows the URL.

![Object ownership](/images/5-Workshop/5.5-Config-public-object/6.png)

7. Verify Public Configuration

Success! Your objects are now publicly accessible.

![Object ownership](/images/5-Workshop/5.5-Config-public-object/7.png)
