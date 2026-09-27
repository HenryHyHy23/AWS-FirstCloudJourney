---
title : "3. Enable S3 Static Website Hosting"
date: 2025-09-09
weight : 3
chapter : false
pre : " <b> 5.3. </b> "
---

In this section, you will enable static website hosting feature from the S3 bucket.

#### Step-by-Step Configuration
1. Access Bucket Properties
    - In your S3 bucket interface, select the **Properties** tab

![Properties](/images/5-Workshop/5.3-S3-static-web/1.png)

2. Find **Static Website Hosting**

In the Properties interface:
- Scroll down to find the **Static website hosting** section
- Select **Edit** to modify settings        
![Static Website Hosting](/images/5-Workshop/5.3-S3-static-web/2.png)

3. Configure Website Hosting Settings

    - **Static website hosting**: Select Enable
    - **Hosting type**: Select Host a static website
    - **Index document**: Enter `index.html`
    - **Error document** (optional): Enter `error.html` if you have a custom error page

![Static Website Hosting](/images/5-Workshop/5.3-S3-static-web/3.png)


4. Save Configuration
![Static Website Hosting](/images/5-Workshop/5.3-S3-static-web/4.png)

### Note:

After enabling static website hosting, your bucket will have a website endpoint URL, but the content will not be accessible until you:
- Upload your website files
- Configure public access
- Set up appropriate bucket policies

5. Verify Configuration
#### What has been achieved
1.  Enabled static website hosting from S3 bucket
2.  Set up website hosting configuration
3.  Obtained website endpoint URL