# Email Testing Guide

Complete procedures for testing Ara's Llantas email templates before sending.

## 📋 Quick Start Testing (5 minutes)

Before every send, perform this quick test:

```bash
# 1. Build the template
npm run build

# 2. Check for errors
npm run test

# 3. Open in browser
open alt-moms-newsletter.html

# 4. Check mobile view
# Use browser DevTools: Ctrl+Shift+I → Device Toolbar → iPhone 12
```

---

## 🧪 Comprehensive Testing Checklist

### Content Verification (10 min)

- [ ] **Spelling & Grammar**
  - Use built-in spell checker
  - Have someone else proofread
  - Check numbers and dates

- [ ] **Template Variables**
  - All {{ }} variables should be REPLACED
  - No blank/missing values
  - Correct format and spelling

- [ ] **Links & URLs**
  - All links begin with `https://`
  - No 404 errors (test each link)
  - UTM parameters present: `?utm_source=newsletter&utm_medium=email&utm_campaign=...`

- [ ] **Phone Numbers & Emails**
  - Phone numbers are clickable: `<a href="tel:+15625551234">`
  - Email addresses are clickable: `<a href="mailto:sales@arasllantas.com">`
  - Format is correct and current

### Design Verification (10 min)

- [ ] **Layout**
  - No horizontal scrolling
  - All sections aligned properly
  - Proper spacing between elements

- [ ] **Images**
  - All images display (don't appear broken)
  - Correct size and aspect ratio
  - Alt text is descriptive
  - Loading time acceptable (<3 sec)

- [ ] **Colors**
  - Black (#000000) displays correctly
  - Orange (#FF6600) is vibrant
  - White backgrounds are clean
  - Grey text is readable (4.5:1 contrast)

- [ ] **Typography**
  - Headlines stand out
  - Body text is readable (12–16px)
  - No text cut off
  - Proper line spacing

- [ ] **Buttons/CTAs**
  - Buttons are clickable
  - Orange color displays correctly
  - Text is readable on button
  - Padding looks right (not crowded)

### Mobile Testing (15 min)

**Using Browser DevTools:**

```
1. Open alt-moms-newsletter.html in Chrome/Firefox
2. Press Ctrl+Shift+I (Windows) or Cmd+Option+I (Mac)
3. Click Device Toolbar icon (top left)
4. Test these sizes:
   - iPhone 12 (390px)
   - iPad (768px)
   - Galaxy S20 (360px)
```

- [ ] **320px width**
  - Text readable without zooming
  - Single column layout
  - No horizontal scrolling
  - Buttons full-width or easily tappable

- [ ] **Mobile UX**
  - Tap targets are 44×44px minimum
  - Links are properly spaced
  - Images scale correctly
  - No element overlap

- [ ] **Portrait & Landscape**
  - Works in both orientations
  - Content reflows properly
  - No scrolling needed for main content

### Email Client Testing (20–30 min)

**Tier 1 - Essential (must test):**

1. **Gmail (Web Version)**
   - Navigate to https://mail.google.com
   - Send yourself a test email
   - Check rendering, images, links

2. **Gmail (Desktop App)**
   - Click the email in Gmail app
   - Verify same rendering as web

3. **Outlook (Web)**
   - Navigate to https://outlook.live.com
   - Send yourself a test email
   - Check rendering

4. **Apple Mail (macOS)**
   - Copy test email to local file
   - Open in Apple Mail
   - Test dark mode rendering

**Tier 2 - Recommended (if resources available):**

5. **Yahoo Mail**
6. **AOL Mail**
7. **Mobile Gmail (iOS/Android)**
8. **Mobile Outlook**

### Accessibility Testing (10 min)

- [ ] **Alt Text**
  - All images have descriptive alt text
  - Alt text makes sense without image
  - No generic labels like "image1" or "picture"

- [ ] **Contrast Ratios**
  - Text vs background has 4.5:1 ratio minimum
  - Links are distinct from regular text
  - Use: https://webaim.org/resources/contrastchecker/

- [ ] **Keyboard Navigation**
  - All links and buttons are keyboard accessible
  - Tab order makes sense
  - Can reach all interactive elements

- [ ] **Dark Mode**
  - Open in dark mode (if client supports)
  - Text remains readable
  - Colors still differentiate elements

### Compliance Testing (5 min)

- [ ] **CAN-SPAM Requirements**
  - [ ] Subject line is clear and non-deceptive
  - [ ] From address is legitimate
  - [ ] Reply-to address works
  - [ ] Physical address in footer ({{ business_address_line_1 }}, etc.)
  - [ ] Clear unsubscribe link ({{ unsubscribe_url }})
  - [ ] Displayed prominently

- [ ] **Security**
  - [ ] All external URLs are HTTPS
  - [ ] Links include `rel="noopener noreferrer"`
  - [ ] No tracking pixels for open rates (or properly disclosed)
  - [ ] No sensitive data in unencrypted email

- [ ] **HTML Validation**
  - Paste HTML into: https://validator.w3.org/
  - Fix any critical errors
  - Warnings OK for email-specific tags

---

## 🔧 Automated Testing

### Using MJML Validation

```bash
# Check syntax
npx mjml --validate alt-moms-newsletter.mjml

# Check for common issues
npx mjml --minimize alt-moms-newsletter.mjml
```

### Email on Acid (Recommended)

**Free Basic Version:**
1. Go to https://www.emailonacid.com
2. Paste HTML in
3. Get previews of major clients

**Paid Version ($):**
- Unlimited tests
- 100+ client previews
- Spam testing
- Analytics integration

### Litmus

**Standard Testing:**
1. Create account at https://www.litmus.com
2. Create test
3. View previews across 90+ clients
4. Check spam score

---

## 📨 Sample Testing Workflow

### Test 1: Local Browser Preview (2 min)

```bash
npm run build
open alt-moms-newsletter.html
# Check in browser, mobile view
```

### Test 2: Variable Replacement (5 min)

Replace template variables:
```javascript
// Example: Node.js replace
const html = fs.readFileSync('alt-moms-newsletter.html', 'utf-8')
  .replace('{{ business_address_line_1 }}', '4521 E Van Buren Street')
  .replace('{{ business_city }}', 'Phoenix')
  .replace('{{ business_state }}', 'AZ')
  .replace('{{ business_zip }}', '85008');
fs.writeFileSync('test-output.html', html);
```

### Test 3: Send to Self (5 min)

Use a test email account or service:

**Option A: Direct SMTP**
```javascript
const nodemailer = require('nodemailer');
const transporter = nodemailer.createTransport({...});

transporter.sendMail({
  from: 'test@arasllantas.com',
  to: 'your-email@gmail.com',
  subject: 'Spring Tire Sale Test',
  html: fs.readFileSync('alt-moms-newsletter.html')
}, (err, info) => {
  if (err) console.error(err);
  else console.log('Sent:', info.response);
});
```

**Option B: Email Service Provider**
- SendGrid: Use API to send test
- Mailgun: Use test domain
- Klaviyo: Use draft campaign

### Test 4: Check in Multiple Clients (10 min)

Open the received email in:
- Gmail (web + app)
- Outlook
- Apple Mail
- (Mobile if possible)

### Test 5: Final QA Checklist (5 min)

Go through **Content Verification** section above one more time.

---

## 🐛 Troubleshooting Common Issues

### "Variables showing as {{ variable }}"

**Cause**: Template not processed by backend
**Solution**: 
- Use Handlebars.js, Jinja2, or ESP template processing
- See `data/variables-guide.md` for examples
- Don't send without replacing variables

### "Images showing broken"

**Cause 1**: Using local paths (`./images/file.jpg`)
**Solution**: Use absolute CDN URL (`https://cdn.arasllantas.com/images/file.jpg`)

**Cause 2**: Image file doesn't exist
**Solution**: Verify file exists and path is correct

**Cause 3**: Email client blocks external images
**Solution**: Provide text fallback and ask user to load images

### "Links aren't working"

**Cause 1**: Missing or malformed URL
**Solution**: Verify URL starts with `https://` and is valid

**Cause 2**: URL has spaces or special characters
**Solution**: URL encode special characters

**Cause 3**: Link has no `href` attribute
**Solution**: Ensure `<a href="...">` syntax is correct

### "Rendering looks different in Outlook"

**Cause**: Outlook uses Word engine, different CSS support
**Solution**: 
- Check `email-clients.md` for Outlook quirks
- MJML automatically adds fixes
- May need VML adjustments for buttons

### "Mobile view is broken"

**Cause**: Viewport meta tag missing
**Solution**: 
- Ensure `<meta name="viewport" content="width=device-width, initial-scale=1">`
- MJML includes this automatically

### "Dark mode looks bad"

**Cause**: Colors inverted but text still readable
**Solution**: 
- Add `color-scheme` meta tags (already included)
- Test in dark mode
- May need image backgrounds for text

---

## ✅ Pre-Send Final Checklist

**Day Before Send:**

- [ ] All content finalized and approved
- [ ] Template variables identified
- [ ] Images uploaded to CDN
- [ ] UTM parameters set correctly
- [ ] Compliance review done

**1 Hour Before Send:**

- [ ] Build latest MJML: `npm run build`
- [ ] Replace all {{ }} variables
- [ ] Send test email to self
- [ ] Check in 3+ email clients
- [ ] Verify all links work
- [ ] Check mobile view
- [ ] Verify unsubscribe link

**5 Minutes Before Send:**

- [ ] Final content check
- [ ] Recipient list verified
- [ ] Send time correct for timezone
- [ ] Everyone has signed off

**During Send:**

- [ ] Monitor bounce rate
- [ ] Check for delivery issues
- [ ] Be ready to halt if problems

**30 Minutes After Send:**

- [ ] Verify delivery succeeded
- [ ] Check email is in recipients' inboxes
- [ ] Not in spam/promotions folder
- [ ] Monitor opens starting

---

## 📊 Post-Send Monitoring

**First Hour:**
- Monitor open rate (should climb quickly)
- Check for immediate bounces
- Monitor complaint rate

**First Day:**
- Track click-through rate
- Monitor unsubscribe rate
- Check customer responses

**Follow-up:**
- Analyze full campaign metrics
- A/B test results if applicable
- Document lessons learned
- Update templates if needed

---

## 📚 Testing Resources

| Tool | Purpose | Cost |
|------|---------|------|
| **Browser DevTools** | Mobile preview | Free |
| **Email on Acid** | Multi-client testing | Free/$$$ |
| **Litmus** | 90+ clients + analytics | $$$ |
| **Validator.w3.org** | HTML validation | Free |
| **WebAIM Contrast** | Accessibility check | Free |
| **MJML CLI** | Local testing & validation | Free |

---

## 🎓 Learning Resources

- **Email Standards**: https://www.emailstandards.org/
- **Campaign Monitor CSS Support**: https://www.campaignmonitor.com/css/
- **Litmus Learning**: https://www.litmus.com/resource-center
- **MJML Docs**: https://mjml.io/documentation
- **Email Accessibility**: https://www.litmus.com/email-accessibility/

---

**Last Updated**: February 25, 2026

**Pro Tip**: Save this checklist and reuse it for every campaign send!
