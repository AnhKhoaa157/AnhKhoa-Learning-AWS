---
title: "Email Service Setup (SES)"
date: "2025-12-06T00:00:00Z"
weight: 4
chapter: false
pre: " <b> 5.7.4. </b> "
---

#### Overview

Amazon SES (Simple Email Service) enables sending transactional emails such as order confirmations, password resets, and user verification emails. In this section, we'll configure SES for the Game Store application.

**What we'll configure:**
- Email address verification in SES
- SMTP credentials for Spring Boot
- Email templates for orders and notifications
- Move out of SES Sandbox (optional)
- Integration with Spring Mail

#### Architecture

```
┌──────────────┐         ┌──────────────┐         ┌──────────────┐
│  Spring Boot │────────►│   SES SMTP   │────────►│   Customer   │
│   Backend    │  Email  │  smtp.server │  Email  │   Inbox      │
└──────────────┘         └──────────────┘         └──────────────┘
      │
      ├─ Order confirmation
      ├─ Password reset
      └─ Account verification
```

#### Step 1: Verify Email Address

SES starts in **Sandbox mode** where you can only send emails to verified addresses.

1. **Navigate to SES Console**
   - Go to: https://console.aws.amazon.com/ses/
   - Region: **ap-southeast-1** (Singapore)

2. **Verify Email Address**
   - Left menu → **Verified identities** → **Create identity**
   - Identity type: ⚪ **Email address**
   - Email address: `your-email@gmail.com` (your actual email)
   - Click **Create identity**

3. **Check Verification Email**
   - Check your inbox for email from `no-reply-aws@amazon.com`
   - Subject: "Amazon SES Email Address Verification Request"
   - Click the verification link
   - Status will change from "Pending" → "Verified"

{{% notice tip %}}
**Multiple Emails:** Verify multiple email addresses for testing (sender and receiver addresses during development).
{{% /notice %}}

#### Step 2: Create SMTP Credentials

Spring Boot uses SMTP to send emails through SES.

1. **Create SMTP Credentials**
   - SES Console → Left menu → **SMTP settings**
   - Click **Create SMTP credentials**
   
2. **IAM User Name**
   - IAM User Name: `game-store-ses-smtp-user`
   - Click **Create user**

3. **Download Credentials**
   - ⚠️ **Download credentials** or copy:
     - SMTP Username: `AKIAZ...` (20 characters)
     - SMTP Password: `BHvs7...` (44 characters)
   - Save these securely!

4. **SMTP Endpoint Information**
   - Server name: `email-smtp.ap-southeast-1.amazonaws.com`
   - Port: `587` (TLS) or `465` (SSL)
   - Authentication: Required (use SMTP credentials)

#### Step 3: Configure Spring Boot Email

1. **Update `application.yaml`**

```yaml
spring:
  mail:
    host: email-smtp.ap-southeast-1.amazonaws.com
    port: 587
    username: ${MAIL_USERNAME}
    password: ${MAIL_PASSWORD}
    properties:
      mail:
        smtp:
          auth: true
          starttls:
            enable: true
            required: true
          connectiontimeout: 5000
          timeout: 5000
          writetimeout: 5000

aws:
  ses:
    from-email: ${AWS_SES_FROM_EMAIL:your-email@gmail.com}
```

2. **Set Environment Variables**

```bash
# Windows PowerShell
$env:MAIL_USERNAME="AKIAZ..."  # Your SMTP username
$env:MAIL_PASSWORD="BHvs7..."  # Your SMTP password
$env:AWS_SES_FROM_EMAIL="your-email@gmail.com"
```

#### Step 4: Email Service Implementation

Your backend already has `EmailService.java` with these methods:

```java
@Service
public class EmailService {
    
    @Autowired
    private JavaMailSender mailSender;
    
    @Value("${aws.ses.from-email}")
    private String fromEmail;
    
    // Send order confirmation email
    public void sendOrderConfirmation(String toEmail, Order order) {
        try {
            MimeMessage message = mailSender.createMimeMessage();
            MimeMessageHelper helper = new MimeMessageHelper(message, true, "UTF-8");
            
            helper.setFrom(fromEmail);
            helper.setTo(toEmail);
            helper.setSubject("Order Confirmation - Order #" + order.getId());
            
            String htmlContent = buildOrderEmail(order);
            helper.setText(htmlContent, true);
            
            mailSender.send(message);
            log.info("Order confirmation email sent to: {}", toEmail);
        } catch (Exception e) {
            log.error("Failed to send email", e);
        }
    }
    
    // Send password reset email
    public void sendPasswordReset(String toEmail, String resetToken) {
        // Implementation
    }
    
    // Send welcome email
    public void sendWelcomeEmail(String toEmail, String username) {
        // Implementation
    }
}
```

#### Step 5: Test Email Sending

1. **Start Backend**
```bash
cd Back-End
mvn spring-boot:run
```

2. **Test via Postman**

**Create Test Order:**
```http
POST http://localhost:8080/identity/orders
Authorization: Bearer YOUR_JWT_TOKEN
Content-Type: application/json

{
    "items": [
        {
            "gameId": 1,
            "quantity": 1
        }
    ],
    "paymentMethod": "MOMO"
}
```

3. **Check Email Inbox**
   - Check the recipient's inbox
   - Look for order confirmation email
   - Verify formatting and content

**Expected Email Content:**
```
Subject: Order Confirmation - Order #12345

Dear Customer,

Thank you for your order!

Order Details:
- Order ID: #12345
- Date: Dec 6, 2025
- Total: $59.99

Items:
1. Game Name - $59.99

We'll send a confirmation when your order ships.

Best regards,
Game Store Team
```

#### Step 6: Email Templates

**Order Confirmation Template:**

```java
private String buildOrderEmail(Order order) {
    StringBuilder html = new StringBuilder();
    html.append("<!DOCTYPE html>");
    html.append("<html><head><meta charset='UTF-8'></head><body>");
    html.append("<div style='font-family: Arial, sans-serif; max-width: 600px; margin: 0 auto;'>");
    
    // Header
    html.append("<h2 style='color: #4CAF50;'>Order Confirmation</h2>");
    html.append("<p>Dear Customer,</p>");
    html.append("<p>Thank you for your order!</p>");
    
    // Order details
    html.append("<div style='background: #f5f5f5; padding: 15px; border-radius: 5px;'>");
    html.append("<h3>Order Details</h3>");
    html.append("<p><strong>Order ID:</strong> #" + order.getId() + "</p>");
    html.append("<p><strong>Date:</strong> " + order.getOrderDate() + "</p>");
    html.append("<p><strong>Total:</strong> $" + order.getTotalAmount() + "</p>");
    html.append("</div>");
    
    // Items
    html.append("<h3>Items:</h3>");
    html.append("<table style='width: 100%; border-collapse: collapse;'>");
    for (OrderDetail item : order.getOrderDetails()) {
        html.append("<tr>");
        html.append("<td>" + item.getGame().getName() + "</td>");
        html.append("<td>x" + item.getQuantity() + "</td>");
        html.append("<td>$" + item.getPrice() + "</td>");
        html.append("</tr>");
    }
    html.append("</table>");
    
    // Footer
    html.append("<p style='margin-top: 20px;'>Best regards,<br>Game Store Team</p>");
    html.append("</div></body></html>");
    
    return html.toString();
}
```

**Password Reset Template:**

```java
private String buildPasswordResetEmail(String resetToken, String username) {
    String resetLink = "https://yourdomain.com/reset-password?token=" + resetToken;
    
    return "<!DOCTYPE html>" +
           "<html><body>" +
           "<h2>Password Reset Request</h2>" +
           "<p>Hello " + username + ",</p>" +
           "<p>Click the link below to reset your password:</p>" +
           "<a href='" + resetLink + "' style='background: #4CAF50; color: white; " +
           "padding: 10px 20px; text-decoration: none; border-radius: 5px;'>" +
           "Reset Password</a>" +
           "<p>This link expires in 1 hour.</p>" +
           "<p>If you didn't request this, please ignore this email.</p>" +
           "</body></html>";
}
```

#### Step 7: SES Sandbox vs Production

**Sandbox Limitations:**
- ✅ Can only send TO verified email addresses
- ✅ Can only send FROM verified email addresses
- ✅ Limit: 200 emails per day
- ✅ Limit: 1 email per second

**Moving Out of Sandbox:**

1. **Request Production Access**
   - SES Console → **Account dashboard**
   - Click **Request production access**

2. **Fill Application Form**
   - Use case: **Transactional emails**
   - Website URL: Your domain
   - Use case description:
     ```
     Our Game Store application sends transactional emails:
     - Order confirmations
     - Password resets
     - Account verification
     - Purchase receipts
     
     Expected volume: ~500 emails/day
     Opt-out process: Unsubscribe link in footer
     Bounce/complaint handling: AWS SNS notifications
     ```

3. **Production Limits**
   - ✅ Send to ANY email address
   - ✅ 50,000 emails per day (can request increase)
   - ✅ 14 emails per second

{{% notice info %}}
**For Development:** Sandbox mode is sufficient. Stay in Sandbox and use verified test emails only.
{{% /notice %}}

#### Step 8: Email Best Practices

**Deliverability:**
- ✅ Use verified domain (not just email)
- ✅ Set up SPF, DKIM, DMARC records
- ✅ Monitor bounce and complaint rates
- ✅ Include unsubscribe link in marketing emails

**Content:**
- ✅ Use clear subject lines
- ✅ Personalize with customer name
- ✅ Mobile-responsive HTML templates
- ✅ Plain text alternative for HTML emails

**Security:**
- ✅ Never include passwords in emails
- ✅ Use secure reset links with tokens
- ✅ Expire reset tokens after 1 hour
- ✅ Rate limit password reset requests

**Example: Domain Verification (Production)**

```bash
# 1. Verify domain in SES Console
# SES Console → Verified identities → Create identity → Domain

# 2. Add DNS records (from SES Console)
# TXT record for verification:
_amazonses.yourdomain.com → "amazonses-verification-token..."

# DKIM records (3 CNAME records):
selector1._domainkey.yourdomain.com → selector1.dkim.amazonses.com
selector2._domainkey.yourdomain.com → selector2.dkim.amazonses.com
selector3._domainkey.yourdomain.com → selector3.dkim.amazonses.com
```

#### Monitoring SES

**CloudWatch Metrics:**
- SES Console → **Reputation dashboard**
- Metrics:
  - Bounce rate (should be <5%)
  - Complaint rate (should be <0.1%)
  - Delivery rate
  - Send rate

**Email Logs:**
```java
// Add logging in EmailService
@Slf4j
@Service
public class EmailService {
    
    public void sendOrderConfirmation(String toEmail, Order order) {
        try {
            // ... send email
            log.info("✅ Email sent successfully to: {}", toEmail);
        } catch (MailException e) {
            log.error("❌ Failed to send email to: {}", toEmail, e);
            // Consider retry logic or dead letter queue
        }
    }
}
```

#### Troubleshooting

**Issue: "Email address is not verified"**

✅ Verify both sender AND recipient in SES Console (Sandbox mode)
✅ Check spam folder for verification email
✅ Ensure email addresses match exactly (case-sensitive)

**Issue: "Daily sending quota exceeded"**

✅ You're in Sandbox mode (200 emails/day limit)
✅ Request production access
✅ Or wait 24 hours for quota reset

**Issue: Emails not arriving**

✅ Check CloudWatch Logs for send errors
✅ Verify SMTP credentials are correct
✅ Check spam/junk folder
✅ Monitor SES bounce/complaint rates
✅ Verify `from-email` is a verified identity

**Issue: "Connection timeout" errors**

✅ Check security group allows outbound port 587
✅ Verify network connectivity
✅ Try port 465 (SSL) instead of 587 (TLS)

**Test SMTP Connection:**
```bash
# Test via telnet
telnet email-smtp.ap-southeast-1.amazonaws.com 587

# Or use openssl
openssl s_client -connect email-smtp.ap-southeast-1.amazonaws.com:587 -starttls smtp
```

#### Cost Estimate

**SES Pricing (ap-southeast-1):**

| Component | Usage | Cost |
|-----------|-------|------|
| Emails sent | 1,000 emails | $0.10/1000 = $0.10 |
| Emails sent | 10,000 emails | $0.10/1000 = $1.00 |
| Attachments | 1GB | $0.12/GB = $0.12 |
| **Total (1000 emails)** | | **~$0.10/month** |

{{% notice info %}}
**Free Tier:** First 62,000 emails per month FREE when sent from EC2!
{{% /notice %}}

#### Summary

You have successfully:
- ✅ Verified email address in SES
- ✅ Created SMTP credentials
- ✅ Configured Spring Boot with SES SMTP
- ✅ Implemented email templates
- ✅ Tested order confirmation emails
- ✅ Understood Sandbox vs Production mode

**Next Step:** Deploy Spring Boot Backend to EC2 →

{{% notice info %}}
**Estimated Time:** 10-15 minutes for SES setup and testing
{{% /notice %}}
