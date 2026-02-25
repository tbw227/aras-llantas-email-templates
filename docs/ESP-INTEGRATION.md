# Email Service Provider (ESP) Integration Guide

Instructions for integrating Ara's Llantas email templates with popular email service providers.

## Quick ESP Comparison

| ESP | Cost | Ease | Features | Best For |
|-----|------|------|----------|----------|
| **Mailgun** | Pay-per-message | Medium | API-first, developer-friendly | Transactional + bulk email |
| **SendGrid** | $100+/month | Medium | Excellent deliverability | Large-scale campaigns |
| **Klaviyo** | Free–$$$$ | Easy | Marketing automation | E-commerce, personalization |
| **Mailchimp** | Free–$$$$ | Very Easy | User-friendly UI | SMB marketing |
| **AWS SES** | Very cheap | Hard | Powerful, scalable | High volume, cost-sensitive |

---

## 📧 Mailgun Integration

### Setup

1. Create account at https://www.mailgun.com
2. Create domain or verify existing domain
3. Get API key from dashboard

### Installation

```bash
npm install mailgun-js
```

### Send Email

```javascript
const mailgun = require('mailgun-js');
const fs = require('fs');

const mg = mailgun({
  apiKey: process.env.MAILGUN_API_KEY,
  domain: process.env.MAILGUN_DOMAIN
});

// Read and process template
let html = fs.readFileSync('alt-moms-newsletter.html', 'utf-8');

// Replace variables
const subscriber = {
  email: 'customer@example.com',
  business_address_line_1: '4521 E Van Buren Street',
  business_city: 'Phoenix',
  business_state: 'AZ',
  business_zip: '85008',
  webview_url: 'https://arasllantas.com/email/view/abc123',
  preferences_url: 'https://arasllantas.com/prefs/xyz789',
  unsubscribe_url: 'https://arasllantas.com/unsub/user123'
};

Object.keys(subscriber).forEach(key => {
  html = html.replace(`{{ ${key} }}`, subscriber[key]);
});

// Send
const msg = {
  from: 'Ara\'s Llantas <noreply@arasllantas.com>',
  to: subscriber.email,
  subject: 'Spring Tire Sale - 20% Off',
  html: html,
  'h:X-Mailgun-Variables': JSON.stringify({
    unsubscribe_url: subscriber.unsubscribe_url
  })
};

mg.messages().send(msg, (error, body) => {
  if (error) {
    console.error('Error:', error);
  } else {
    console.log('Email sent:', body.id);
  }
});
```

### Batch Send

```javascript
// Send to multiple recipients
const subscribers = [
  { email: 'user1@example.com', ... },
  { email: 'user2@example.com', ... },
];

subscribers.forEach(subscriber => {
  // Process and send (code above)
});
```

---

## 🚀 SendGrid Integration

### Setup

1. Create account at https://sendgrid.com
2. Create API key (Settings → API Keys)
3. Save key to `.env`

### Installation

```bash
npm install @sendgrid/mail
```

### Send Email

```javascript
const sgMail = require('@sendgrid/mail');
sgMail.setApiKey(process.env.SENDGRID_API_KEY);

const fs = require('fs');

// Read and process template
let html = fs.readFileSync('alt-moms-newsletter.html', 'utf-8');

const subscriber = {
  email: 'customer@example.com',
  business_address_line_1: '4521 E Van Buren Street',
  business_city: 'Phoenix',
  business_state: 'AZ',
  business_zip: '85008',
  webview_url: 'https://arasllantas.com/email/view/abc123',
  preferences_url: 'https://arasllantas.com/prefs/xyz789',
  unsubscribe_url: 'https://arasllantas.com/unsub/user123'
};

// Replace variables
Object.keys(subscriber).forEach(key => {
  html = html.replace(`{{ ${key} }}`, subscriber[key]);
});

// Send
const msg = {
  to: subscriber.email,
  from: 'noreply@arasllantas.com',
  subject: 'Spring Tire Sale - 20% Off',
  html: html,
  replyTo: 'sales@arasllantas.com',
  trackingSettings: {
    clickTracking: {
      enable: true
    },
    openTracking: {
      enable: true
    }
  }
};

sgMail.send(msg)
  .then(() => console.log('Email sent'))
  .catch(error => console.error('Error:', error));
```

---

## 📱 Klaviyo Integration

### Setup

1. Create account at https://www.klaviyo.com
2. Create API key (Business Settings → API Keys)
3. Paste template into Klaviyo email editor

### Using Klaviyo Template Editor

1. **Create Campaign** → New Email
2. **Design** → Enter Code (paste HTML)
3. **Replace Klaviyo variables:**

```html
<!-- Mailchimp variables conversion -->
{{ business_address_line_1 }} → [[ business_address_line_1 ]]
{{ business_city }} → [[ business_city ]]
<!-- etc. -->
```

### Sample API Call (advanced)

```javascript
const fetch = require('node-fetch');

async function sendKlaviyoEmail(subscriber) {
  const API_KEY = process.env.KLAVIYO_API_KEY;
  const LIST_ID = process.env.KLAVIYO_LIST_ID;

  // Add subscriber to list
  const response = await fetch(
    `https://a.klaviyo.com/api/v1/list/${LIST_ID}/members`,
    {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        api_key: API_KEY,
        profiles: [
          {
            email: subscriber.email,
            properties: {
              business_address_line_1: subscriber.business_address_line_1,
              business_city: subscriber.business_city,
              business_state: subscriber.business_state,
              business_zip: subscriber.business_zip
            }
          }
        ]
      })
    }
  );

  return response.json();
}
```

---

## 📧 Mailchimp Integration

### Setup

1. Create account at https://mailchimp.com
2. Install npm package: `npm install @mailchimp/marketing`
3. Get API key from account settings

### Send Email

```javascript
const mailchimp = require('@mailchimp/marketing');

mailchimp.setConfig({
  apiKey: process.env.MAILCHIMP_API_KEY,
  server: process.env.MAILCHIMP_SERVER_PREFIX // e.g., 'us1'
});

async function sendMailchimpEmail(subscriber) {
  try {
    const response = await mailchimp.lists.setListMember(
      process.env.MAILCHIMP_AUDIENCE_ID,
      subscriber.email,
      {
        email_address: subscriber.email,
        status_if_new: 'pending',
        merge_fields: {
          ADDRESS1: subscriber.business_address_line_1,
          CITY: subscriber.business_city,
          STATE: subscriber.business_state,
          ZIP: subscriber.business_zip
        }
      }
    );
    console.log('Subscriber added:', response);
  } catch (error) {
    console.error('Error:', error);
  }
}
```

---

## ☁️ AWS SES Integration

### Setup

1. Create AWS account
2. Request production access for SES
3. Verify domain (DKIM + SPF)
4. Create IAM user with SES permissions
5. Save access key + secret

### Installation

```bash
npm install aws-sdk
```

### Send Email

```javascript
const AWS = require('aws-sdk');

AWS.config.update({
  accessKeyId: process.env.AWS_ACCESS_KEY_ID,
  secretAccessKey: process.env.AWS_SECRET_ACCESS_KEY,
  region: 'us-east-1'
});

const ses = new AWS.SES({ apiVersion: '2010-12-01' });

async function sendAWSEmail(subscriber) {
  const fs = require('fs');
  
  // Read template
  let html = fs.readFileSync('alt-moms-newsletter.html', 'utf-8');
  
  // Replace variables
  Object.keys(subscriber).forEach(key => {
    html = html.replace(`{{ ${key} }}`, subscriber[key]);
  });

  const params = {
    Source: 'noreply@arasllantas.com',
    Destination: {
      ToAddresses: [subscriber.email]
    },
    Message: {
      Subject: {
        Data: 'Spring Tire Sale - 20% Off'
      },
      Body: {
        Html: {
          Data: html
        }
      }
    }
  };

  try {
    const result = await ses.sendEmail(params).promise();
    console.log('Email sent:', result.MessageId);
  } catch (error) {
    console.error('Error:', error);
  }
}
```

---

## 🔧 Generic Template Processing

### Universal Template Replacer Function

```javascript
function processTemplate(htmlContent, variables) {
  let result = htmlContent;
  
  // Replace all {{ variable }} patterns
  Object.keys(variables).forEach(key => {
    const pattern = new RegExp(`{{\\s*${key}\\s*}}`, 'g');
    result = result.replace(pattern, variables[key]);
  });
  
  // Check for unreplaced variables
  const unreplaced = result.match(/{{[^}]+}}/g);
  if (unreplaced) {
    console.warn('Unreplaced variables:', unreplaced);
  }
  
  return result;
}

// Usage
const template = fs.readFileSync('alt-moms-newsletter.html', 'utf-8');
const variables = {
  business_address_line_1: '4521 E Van Buren Street',
  business_city: 'Phoenix',
  business_state: 'AZ',
  business_zip: '85008',
  webview_url: 'https://arasllantas.com/email/view/abc123',
  preferences_url: 'https://arasllantas.com/prefs/xyz789',
  unsubscribe_url: 'https://arasllantas.com/unsub/user123'
};

const finalHTML = processTemplate(template, variables);
```

---

## 📊 Best Practices

### Before Sending

✅ **DO:**
- Test with single recipient first
- Verify all variables replaced
- Check deliverability score
- Monitor bounce rate
- Set up bounce/complaint handling
- Enable click/open tracking (if desired)

❌ **DON'T:**
- Send to test addresses (affects statistics)
- Use demo/sandbox mode in production
- Ignore authentication (SPF, DKIM, DMARC)
- Send large batches without testing

### After Sending

- Monitor delivery rate
- Check for spam complaints
- Review open/click rates
- Identify undeliverable addresses
- Unsubscribe bounced addresses

---

## 🔐 Security Best Practices

### Protect API Keys

```javascript
// Use environment variables
const API_KEY = process.env.ESP_API_KEY;

// Never commit to git
// Add to .gitignore and .env.local
```

### Validate Email Addresses

```javascript
function isValidEmail(email) {
  return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
}
```

### Rate Limiting

```javascript
// Avoid sending too fast
const delay = (ms) => new Promise(resolve => setTimeout(resolve, ms));

for (const subscriber of subscribers) {
  await sendEmail(subscriber);
  await delay(1000); // 1 second between emails
}
```

---

## 📈 Monitoring & Analytics

### Track Key Metrics

- **Delivery Rate**: % emails delivered successfully
- **Bounce Rate**: % hard/soft bounces
- **Complaint Rate**: % marked as spam (<0.5% target)
- **Open Rate**: % emails opened (15-25% is good)
- **Click Rate**: % links clicked (2-5% is good)

### Set Up Alerts

Configure email service to alert on:
- High bounce rate (>5%)
- Spam complaints (>0.1%)
- Delivery failures

---

## 📚 Resources

- **Mailgun Docs**: https://documentation.mailgun.com/
- **SendGrid Docs**: https://sendgrid.com/docs/
- **Klaviyo Docs**: https://developers.klaviyo.com/
- **AWS SES Docs**: https://docs.aws.amazon.com/ses/
- **Email Deliverability**: https://sendgrid.com/blog/email-deliverability/

---

**Last Updated**: February 25, 2026
