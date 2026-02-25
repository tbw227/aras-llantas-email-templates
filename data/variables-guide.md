# Template Variables Guide

Complete reference for all template variables used in Ara's Llantas email templates.

## Core Navigation Variables

### Email Link Variables
These URLs allow subscribers to interact with the email and manage their preferences.

| Variable | Purpose | Required | Example |
|----------|---------|----------|---------|
| `{{ webview_url }}` | View email in browser | Yes | `https://arasllantas.com/email/view/abc123` |
| `{{ preferences_url }}` | Email preference center | Yes | `https://arasllantas.com/preferences/user123` |
| `{{ unsubscribe_url }}` | Unsubscribe link (CAN-SPAM compliant) | **Required by law** | `https://arasllantas.com/unsubscribe/user123` |

## Business Information Variables

Used in footer and header for company branding.

| Variable | Purpose | Example |
|----------|---------|---------|
| `{{ business_address_line_1 }}` | Street address | 4521 E Van Buren Street |
| `{{ business_city }}` | City | Phoenix |
| `{{ business_state }}` | State abbreviation | AZ |
| `{{ business_zip }}` | ZIP code | 85008 |
| `{{ business_phone }}` | Phone number | (602) 555-TIRE |
| `{{ business_email }}` | Contact email | sales@arasllantas.com |

## Campaign Variables

Campaign-specific data for promotions and tracking.

| Variable | Purpose | Example |
|----------|---------|---------|
| `{{ campaign_name }}` | Campaign identifier | spring-tire-sale-2026 |
| `{{ promo_code }}` | Discount/promo code | SPRINGTIRE20 |
| `{{ discount_percent }}` | Discount percentage | 20 |
| `{{ offer_end_date }}` | Promotion expiration | March 31, 2026 |

## Analytics & Tracking Variables

Used in URLs for campaign tracking.

| Variable | Purpose | Example |
|----------|---------|---------|
| `{{ utm_source }}` | Analytics source | newsletter |
| `{{ utm_medium }}` | Analytics medium | email |
| `{{ utm_campaign }}` | Analytics campaign | spring-tire-sale |

---

## Using Variables in MJML

### Syntax
```mjml
<mj-text>Your text with {{ variable_name }} interpolation</mj-text>
<mj-button href="https://example.com?promo={{ promo_code }}">Click here</mj-button>
```

### Example Email Section
```mjml
<mj-section background-color="#ffffff" padding="20px">
  <mj-column>
    <mj-text font-size="16px">
      Get {{ discount_percent }}% off our premium tire selection through {{ offer_end_date }}
    </mj-text>
    <mj-button href="https://arasllantas.com/shop?code={{ promo_code }}">
      Claim Your Discount
    </mj-button>
  </mj-column>
</mj-section>
```

## Processing Template Variables

Your backend must replace these variables before sending. Examples below:

### Node.js with Handlebars
```javascript
const Handlebars = require('handlebars');
const fs = require('fs');

const htmlContent = fs.readFileSync('alt-moms-newsletter.html', 'utf-8');
const template = Handlebars.compile(htmlContent);

const data = {
  webview_url: 'https://arasllantas.com/email/view/abc123',
  preferences_url: 'https://arasllantas.com/preferences/user123',
  unsubscribe_url: 'https://arasllantas.com/unsubscribe/user123',
  business_address_line_1: '4521 E Van Buren Street',
  business_city: 'Phoenix',
  business_state: 'AZ',
  business_zip: '85008',
  promo_code: 'SPRINGTIRE20',
  discount_percent: 20,
  offer_end_date: 'March 31, 2026'
};

const finalHTML = template(data);
```

### Python with Jinja2
```python
from jinja2 import Template

with open('alt-moms-newsletter.html', 'r') as f:
    html_content = f.read()

template = Template(html_content)

data = {
    'webview_url': 'https://arasllantas.com/email/view/abc123',
    'preferences_url': 'https://arasllantas.com/preferences/user123',
    'unsubscribe_url': 'https://arasllantas.com/unsubscribe/user123',
    'business_address_line_1': '4521 E Van Buren Street',
    'business_city': 'Phoenix',
    'business_state': 'AZ',
    'business_zip': '85008',
    'promo_code': 'SPRINGTIRE20',
    'discount_percent': 20,
    'offer_end_date': 'March 31, 2026'
}

final_html = template.render(data)
```

### PHP with Twig
```php
require_once 'vendor/autoload.php';

$loader = new \Twig\Loader\FileSystemLoader('/path/to/templates');
$twig = new \Twig\Environment($loader);

$data = [
    'webview_url' => 'https://arasllantas.com/email/view/abc123',
    'preferences_url' => 'https://arasllantas.com/preferences/user123',
    'unsubscribe_url' => 'https://arasllantas.com/unsubscribe/user123',
    'business_address_line_1' => '4521 E Van Buren Street',
    'business_city' => 'Phoenix',
    'business_state' => 'AZ',
    'business_zip' => '85008',
    'promo_code' => 'SPRINGTIRE20',
    'discount_percent' => 20,
    'offer_end_date' => 'March 31, 2026'
];

echo $twig->render('alt-moms-newsletter.html', $data);
```

## Variable Replacement via Email Service Providers (ESPs)

Most ESPs handle variable replacement automatically. Examples:

### Mailgun
```javascript
const msg = {
  from: 'noreply@arasllantas.com',
  to: '%recipient.email%',
  subject: 'Spring Tire Sale',
  html: htmlContent,
  'h:X-Mailgun-Variables': JSON.stringify({
    'unsubscribe_url': `https://arasllantas.com/unsubscribe/${recipientId}`,
    'promo_code': 'SPRINGTIRE20'
  })
};
```

### SendGrid
```javascript
const msg = {
  to: 'recipient@example.com',
  from: 'noreply@arasllantas.com',
  subject: 'Spring Tire Sale',
  html: htmlContent,
  substitutions: {
    '-unsubscribe_url-': 'https://arasllantas.com/unsubscribe/user123',
    '-promo_code-': 'SPRINGTIRE20'
  }
};
```

### Klaviyo
```json
{
  "email": "subscriber@example.com",
  "subject_variables": {
    "promo_code": "SPRINGTIRE20"
  },
  "resource": {
    "type": "template-html",
    "id": "spring-tire-sale"
  }
}
```

---

## Best Practices

✅ **DO:**
- Always URL-encode variables when used in links: `{{ promo_code | urlencode }}`
- Provide default values for optional variables
- Test all variable combinations before sending
- Use consistent variable naming across all templates
- Document any custom variables you add

❌ **DON'T:**
- Use special characters in variable names (stick to alphanumeric + underscore)
- Forget the curly braces: use `{{ variable }}` NOT `variable`
- Mix template languages (don't use `${}` or `[]` syntax)
- Send emails without testing variable replacement
- Hardcode values that should be variables (defeats personalization)

---

For more information, see [README.md](../README.md) or the main project documentation.
