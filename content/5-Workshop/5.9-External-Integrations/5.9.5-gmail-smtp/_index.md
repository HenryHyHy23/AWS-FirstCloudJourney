---
title: "Send a test OTP through Gmail SMTP"
menuTitle: "Gmail SMTP for System OTP"
weight: 5
chapter: false
pre: "<b> 5.9.5. </b> "
---

1. **Open the lab Gmail account authorized to send mail.** If policy allows app passwords, enable 2-Step Verification and generate an app password. In Workspace use the administrator-approved authentication method.

2. **Open the backend secret and configure Spring Mail.** Use the following for STARTTLS.
   - Host — smtp.gmail.com
   - Port — 587
   - SMTP auth and STARTTLS — enabled
   - Username and credential — inject with ECS ValueFrom using the application variable names

3. **Deploy fresh tasks and open activation or password reset in BTB.** Use a demo account and a separate recipient mailbox. Record the send time, check inbox/spam and enter the code in the app.

**Verify.** The code works within its validity period and follows the application's single-use rules. For authentication failure check credentials. For timeout check outbound SMTP connectivity. Mask the OTP when capturing evidence.
