---
title : "Configure CloudFront"
date: 2025-09-09
weight : 2
chapter : false
pre : " <b> 5.7.2 </b> "
---

Use AWS Management Console to create a **CloudFront distribution** and configure this service to serve the **S3 Bucket** we created earlier.

1. Open CloudFront Console
    -   In AWS Console, search for **CloudFront**

    ![CloudFront](/images/5-Workshop/5.7-Cloudfront/5.7.2-Config-cloudfront/1.png)

    From the dashboard, click on Create a CloudFront distribution.
    ![CloudFront](/images/5-Workshop/5.7-Cloudfront/5.7.2-Config-cloudfront/2.png)

2. Configure CloudFront
    -   In the plan selection interface, choose **Free plan** (note: this step might vary in newer console versions)
    -   Select Next
    ![CloudFront](/images/5-Workshop/5.7-Cloudfront/5.7.2-Config-cloudfront/3.png)

    -   Distribution name: custom name (or `workshop-demo`)
    -   Distribution type: select **Single website or app**
    -   Select Next
    ![CloudFront](/images/5-Workshop/5.7-Cloudfront/5.7.2-Config-cloudfront/4.png)

    - On Specify Origin page
        -   Origin type: select **Amazon S3**
    ![CloudFront](/images/5-Workshop/5.7-Cloudfront/5.7.2-Config-cloudfront/5.png)
         -   Origin: select created **S3 bucket**
         -  Click **Choose**
    ![CloudFront](/images/5-Workshop/5.7-Cloudfront/5.7.2-Config-cloudfront/6.png)
        -   Then click Next
    ![CloudFront](/images/5-Workshop/5.7-Cloudfront/5.7.2-Config-cloudfront/7.png)
        -   Continue Next
    ![CloudFront](/images/5-Workshop/5.7-Cloudfront/5.7.2-Config-cloudfront/8.png)
        -   Review configuration, and then select **Create distribution**
    ![CloudFront](/images/5-Workshop/5.7-Cloudfront/5.7.2-Config-cloudfront/9.png)

Then wait a few minutes for CloudFront to initialize

![CloudFront](/images/5-Workshop/5.7-Cloudfront/5.7.2-Config-cloudfront/10.png)

Go to S3, into **Permissions** and scroll down to find **Bucket policy**
![CloudFront](/images/5-Workshop/5.7-Cloudfront/5.7.2-Config-cloudfront/11.png)


#### Optional: If not working, try the following methods:
1. Turn off S3 Static Website Hosting
    ![CloudFront](/images/5-Workshop/5.7-Cloudfront/5.7.2-Config-cloudfront/12.png)  
    ![CloudFront](/images/5-Workshop/5.7-Cloudfront/5.7.2-Config-cloudfront/13.png)   
2. Then go to configure CloudFront
    -   Click on CloudFront **ID**
    ![CloudFront](/images/5-Workshop/5.7-Cloudfront/5.7.2-Config-cloudfront/14.png)

    -   In **General**, select **Edit**
    -   Add `index.html` to **Default root object**
    -   Then select **Save changes**
    ![CloudFront](/images/5-Workshop/5.7-Cloudfront/5.7.2-Config-cloudfront/15.png)

Then wait a few more minutes for CloudFront to re-initialize, and try opening the URL again in a new tab (or new browser)
    ![CloudFront](/images/5-Workshop/5.7-Cloudfront/5.7.2-Config-cloudfront/16.png)
