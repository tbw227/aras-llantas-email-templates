# Ara's Llantas Email Templates

Professional email marketing templates for Ara's Llantas lawn mower tire promotions and campaigns.

## 📋 Project Structure

```
music_news/
├── aras-llantas-spring-promo.mjml  # MJML source template
├── aras-llantas-spring-promo.html  # Compiled HTML output
├── package.json                   # Project dependencies & scripts
├── .env.example                   # Environment variables template
├── .gitignore                     # Git ignore rules
├── README.md                      # This file
├── data/
│   ├── sample-data.json           # Test data for template rendering
│   └── variables-guide.md         # Template variable documentation
├── images/                        # Email images & assets
├── css/                           # Standalone CSS files
├── docs/
│   ├── best-practices.md          # Email development best practices
│   ├── email-clients.md           # Email client compatibility
│   └── testing-guide.md           # Testing procedures
└── src/
    └── templates/                 # Source MJML templates
```

## 🚀 Quick Start

### Prerequisites
- **Node.js** 14+ ([Download](https://nodejs.org/))
- **npm** or **yarn**

### Installation

```bash
# Install dependencies
npm install

# Or with yarn
yarn install
```

### Development

```bash
# Watch mode - auto-compile on changes
npm run watch

# Build once
npm run build

# Test email content
npm run test
```

## 📧 Template Variables

The email template uses template variables that need to be populated before sending. Create a `.env` file based on `.env.example`:

```bash
cp .env.example .env
# Edit .env with your actual values
```

### Available Variables

| Variable | Type | Description |
|----------|------|-------------|
| `{{ webview_url }}` | URL | Link to view email in browser |
| `{{ preferences_url }}` | URL | Email preference center link |
| `{{ unsubscribe_url }}` | URL | Unsubscribe link (required by law) |
| `{{ business_address_line_1 }}` | Text | Street address |
| `{{ business_city }}` | Text | City name |
| `{{ business_state }}` | Text | State abbreviation |
| `{{ business_zip }}` | Text | ZIP code |

## 🎨 Brand Colors

- **Black**: #000000 (Primary)
- **Orange**: #FF6600 (Accent/CTA)
- **Grey**: #666666–#777777 (Secondary text)
- **White**: #FFFFFF (Backgrounds)

## 📧 Compiling MJML to HTML

MJML is an email markup language that compiles to responsive HTML. Our build process handles this automatically:

```bash
# Watch for changes and auto-compile
npm run watch

# Manual compile
npm run build
```

**Output**: `aras-llantas-spring-promo.html` (ready to send)

## ✅ Email Client Compatibility

This template is tested and compatible with:

- ✅ Gmail (desktop, mobile, app)
- ✅ Outlook (2016+, Web, Mobile)
- ✅ Apple Mail (macOS, iOS)
- ✅ Yahoo Mail
- ✅ AOL Mail
- ✅ Mobile clients (iOS Mail, Android Gmail)
- ✅ Dark mode support

For detailed compatibility info, see [docs/email-clients.md](docs/email-clients.md).

## 📸 Images

Store all email images in the `images/` folder. For production:

1. **Optimize images**: Use tools like TinyPNG or ImageOptim
2. **Use CDN**: Upload to a content delivery network (S3, Cloudflare)
3. **Update paths**: Change local paths to absolute URLs in `alt-moms-newsletter.mjml`

```mjml
<!-- Before (local) -->
<mj-image src="./images/mower-tires.jpg" />

<!-- After (production) -->
<mj-image src="https://cdn.example.com/images/mower-tires.jpg" />
```

## 🧪 Testing

### Pre-Send Checklist

- [ ] All template variables replaced with actual values
- [ ] Links are valid and tracked (UTM parameters)
- [ ] Images load correctly (use absolute URLs)
- [ ] No spelling/grammar errors
- [ ] Mobile preview looks correct (600px width)
- [ ] PDF render test (email to PDF)
- [ ] Dark mode preview
- [ ] Test in target email clients

### Testing Tools

1. **Email on Acid** (www.emailonacid.com) - Multi-client testing
2. **Litmus** (www.litmus.com) - Preview & analytics
3. **MJML Check** - Built-in validation
4. **Mailmodo** - Interactive email testing

## 🔐 Security Best Practices

✅ **Implemented:**
- `rel="noopener noreferrer"` on external links
- `target="_blank"` for safe link handling
- Proper meta tags for dark mode & color scheme
- Clear unsubscribe option (CAN-SPAM compliant)

⚠️ **Before Sending:**
- Verify all URLs are HTTPS
- Test unsubscribe mechanism
- Ensure compliance with CAN-SPAM Act
- Add proper authentication (SPF, DKIM, DMARC)

## 📊 Analytics & Tracking

All CTAs include UTM parameters for campaign tracking:

```
?utm_source=newsletter&utm_medium=email&utm_campaign=spring-tire-sale
```

Update `CAMPAIGN_NAME` in `.env` to track different campaigns.

## 🔄 Workflow

1. **Edit Template**: Modify `alt-moms-newsletter.mjml`
2. **Auto-Compile**: Watch mode compiles to HTML automatically
3. **Preview**: Open `.html` in browser or email client
4. **Test**: Run full test suite
5. **Deploy**: Send via email service provider (ESP)
6. **Monitor**: Track opens, clicks, unsubscribes

## 📧 Sending via Email Service Providers

### Mailgun
```javascript
const mailgun = require('mailgun-js');
const fs = require('fs');

const mg = mailgun({ apiKey: process.env.ESP_API_KEY, domain: process.env.ESP_DOMAIN });
const htmlContent = fs.readFileSync('alt-moms-newsletter.html', 'utf-8');

mg.messages().send({
  from: 'Ara\'s Llantas <noreply@arasllantas.com>',
  to: 'subscriber@example.com',
  subject: 'Lawn Mower Season Special - 20% Off Tires',
  html: htmlContent
}, (error, body) => {
  if (error) console.error(error);
  else console.log('Email sent!', body);
});
```

### SendGrid
```javascript
const sgMail = require('@sendgrid/mail');
sgMail.setApiKey(process.env.SENDGRID_API_KEY);

const msg = {
  to: 'recipient@example.com',
  from: 'noreply@arasllantas.com',
  subject: 'Lawn Mower Season Special',
  html: fs.readFileSync('alt-moms-newsletter.html', 'utf-8'),
};

sgMail.send(msg);
```

## 🛠️ Customization

### Changing Colors

Edit `alt-moms-newsletter.mjml`:

```mjml
<mj-button background-color="#FF6600">  <!-- Change to your color -->
```

### Updating Content

- **Header**: `<mj-text>` elements in the first `<mj-section>`
- **Main CTA**: Update button `href` and text
- **Footer**: Update business info and links

### Adding Sections

Copy and modify existing `<mj-section>` blocks:

```mjml
<mj-section background-color="#ffffff" padding="20px">
  <mj-column>
    <mj-text font-size="22px" font-weight="700">Section Title</mj-text>
  </mj-column>
</mj-section>
```

## 📚 Resources

- **MJML Docs**: https://mjml.io/
- **Email Standards**: https://www.campaignmonitor.com/guides/
- **CAN-SPAM Act**: https://www.ftc.gov/business-guidance/pages/can-spam-act-compliance-guide
- **Email Accessibility**: https://www.litmus.com/email-accessibility/

## 🐛 Troubleshooting

### Image Not Loading
- Use absolute URL (HTTPS)
- Check file exists and is accessible
- Verify image dimensions

### Template Variables Not Replacing
- Ensure variables are wrapped in `{{ }}`
- Backend must process template before sending
- Check ESP documentation

### Rendering Issues in Specific Client
- Check [docs/email-clients.md](docs/email-clients.md)
- Test in Email on Acid or Litmus
- Use client-specific CSS fixes

## 📝 License

MIT License - Feel free to use and modify these templates.

## 👥 Contributing

- Keep templates responsive (mobile-first)
- Test changes in target email clients
- Update documentation for any changes
- Follow WCAG accessibility guidelines

---

**Last Updated**: February 25, 2026  
**Maintained by**: Ara's Llantas  
**Questions?** Contact: support@arasllantas.com
