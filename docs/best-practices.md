# Email Development Best Practices

Professional standards and best practices for Ara's Llantas email templates.

## 📋 Email Standards Compliance

### CAN-SPAM Act (USA)
Every email **MUST** include:
- ✅ Clear, honest subject line
- ✅ Physical mailing address in footer ({{ business_address_line_1 }}, etc.)
- ✅ Clear, functioning unsubscribe link: `{{ unsubscribe_url }}`
- ✅ Honor unsubscribe requests within 10 business days
- ✅ Clear identification as advertisement if promotional

**Penalty**: Up to $43,280 per violation.

### GDPR (European Union)
- ✅ Only send to users with **explicit opt-in consent**
- ✅ Provide easy unsubscribe (already included)
- ✅ Honor data requests within 30 days
- ✅ Document consent method and date
- ✅ Use double opt-in for new subscribers (recommended)

**Penalty**: Up to 4% of global annual revenue.

---

## 🎨 Design Best Practices

### Responsive Design
- **Mobile-first**: Design for mobile (320px) first, then scale up
- **Width**: Keep max-width at 600px (email client standard)
- **Padding**: Minimum 16px on sides for mobile readability
- **Font sizes**: Minimum 14px for body text
- **Line height**: 1.6 for better readability

### Color Palette
- **Primary**: Black (#000000) for structure/text
- **Accent**: Orange (#FF6600) for CTAs and highlights
- **Secondary**: Grey (#666666–#777777) for supporting text
- **Background**: White (#FFFFFF) for clarity

**Accessibility**: Ensure 4.5:1 contrast ratio minimum (WCAG AA)

### Typography
```css
/* Body text */
font-family: Arial, Helvetica, sans-serif;
font-size: 16px;
line-height: 1.6;
color: #000000;

/* Headlines */
font-size: 22px–28px;
font-weight: 700;
line-height: 1.3;

/* Secondary text */
font-size: 12px–14px;
color: #666666;
```

### Buttons (CTAs)
- **Background**: Orange (#FF6600)
- **Text**: White (#FFFFFF)
- **Padding**: 12px 20px minimum
- **Border-radius**: 4px for slightly rounded corners
- **Width**: 100% on mobile, auto on desktop
- **Click area**: Minimum 44px × 44px (mobile accessibility)

```mjml
<mj-button 
  href="https://arasllantas.com/shop?utm_source=newsletter&utm_campaign=spring-sale"
  background-color="#FF6600"
  color="#ffffff"
  border-radius="4px"
  font-size="15px"
  font-weight="700"
  padding="12px 20px"
>
  Shop Now
</mj-button>
```

---

## 🖼️ Image Optimization

### File Formats
- **JPEG**: Photos and complex images
- **PNG**: Icons, logos, transparent images
- **GIF**: Animated graphics (sparingly)

### Optimization
- **JPEG**: Max 150KB per image
- **PNG**: Max 100KB per image
- **Use compression**: TinyPNG, ImageOptim
- **Dimensions**: Never larger than 600px wide
- **Resolution**: 72dpi (email standard)

### Alt Text
Every image **must** have descriptive alt text:
```mjml
<mj-image
  src="https://cdn.arasllantas.com/images/tire-promotion.jpg"
  alt="Premium lawn mower tires with 20% spring discount"
/>
```

### Fallback for Non-Image Loading
```mjml
<!-- Provide text if images don't load -->
<mj-text font-size="16px">
  [Spring tire promotion details text here for users who don't load images]
</mj-text>
```

---

## 📱 Link & Tracking Best Practices

### Link Security
All external links should be HTTPS and include security attributes:
```html
<a href="https://arasllantas.com/shop" 
   target="_blank" 
   rel="noopener noreferrer"
   style="color:#FF6600;">
  Shop Now
</a>
```

**Attributes explained:**
- `target="_blank"`: Opens in new tab/window
- `rel="noopener"`: Prevents window.opener access (security)
- `rel="noreferrer"`: Hides referrer information (privacy)

### UTM Tracking
Add UTM parameters to all links for analytics:
```
?utm_source=newsletter&utm_medium=email&utm_campaign=spring-tire-sale
```

**Full example:**
```
https://arasllantas.com/shop?utm_source=newsletter&utm_medium=email&utm_campaign=spring-tire-sale&utm_content=hero-button
```

**Parameters:**
- `utm_source`: Where traffic comes from (always "newsletter")
- `utm_medium`: Channel type (always "email")
- `utm_campaign`: Campaign name (descriptive)
- `utm_content`: Which specific link/button (optional)

---

## ✨ Content Best Practices

### Subject Lines
- **Length**: 40–50 characters optimal
- **Preview**: First 40 chars visible in preview
- **Avoid**: ALL CAPS, excessive punctuation, spam words
- **Test**: Include personalization when possible

```
❌ BAD:  "GET YOUR TIRES NOW!!! Spring Blowout Sale $$$"
✅ GOOD: "Spring Tire Sale: 20% Off Your Next Purchase"
✅ BETTER: "Spring waiting? Get 20% off tires for {{ business_city }}"
```

### Preheader Text
Hidden preview text that appears in email client:
```mjml
<mj-preview>Get your lawn mower ready! Premium tires for the season at Ara's Llantas – save today.</mj-preview>
```

**Best practices:**
- 40–120 characters
- Summarize email value
- Complement subject line
- Don't repeat subject

### Body Copy
- **Opening**: Hook readers in first sentence
- **Length**: Keep sections to 2–3 sentences
- **Hierarchy**: Use headings, short paragraphs
- **White space**: 20px+ padding between sections
- **Calls-to-action**: 1–2 primary CTAs per email

```
❌ BAD (wall of text):
"We have many tires available in different sizes for your lawn mower and we think you should buy them because they are high quality and on sale this month."

✅ GOOD (clear structure):
"Spring is here! Get your lawn mower ready.

Our premium tire selection is 20% off through March 31st.

✓ Proven durability
✓ Better traction
✓ Free installation on orders over $100

Shop Now"
```

### Social Proof Elements
- Include testimonials (with permission)
- Highlight recent promotions/offers
- Show product ratings/reviews
- Use customer success stories

---

## 🧪 Testing Checklist

Before every send:

**[ ] Content & Copy**
- [ ] No spelling or grammar errors
- [ ] All template variables replaced
- [ ] Links are valid and tracked (UTM parameters)
- [ ] Phone numbers are clickable: `<a href="tel:+1234567890">`
- [ ] Email addresses are clickable: `<a href="mailto:email@example.com">`

**[ ] Design & Display**
- [ ] Responsive on mobile (test at 320px, 480px, 600px)
- [ ] All images load correctly
- [ ] Colors render accurately
- [ ] Buttons are clickable and have proper padding
- [ ] No text is cut off or overlapping

**[ ] Email Clients**
- [ ] Test in: Gmail, Outlook, Apple Mail, Yahoo Mail, mobile iOS/Android
- [ ] Check dark mode rendering
- [ ] Verify in web-based clients

**[ ] Compliance & Security**
- [ ] Unsubscribe link works
- [ ] Privacy policy link accessible
- [ ] No malicious scripts
- [ ] HTTPS on all external URLs
- [ ] rel="noopener noreferrer" on target="_blank" links

**[ ] Analytics**
- [ ] UTM parameters present
- [ ] Campaign name set correctly
- [ ] Test conversion tracking

---

## 🔒 Security Best Practices

### Never Include
- ❌ Credit card information
- ❌ Social Security numbers
- ❌ Passwords or tokens
- ❌ Unencrypted personal data
- ❌ JavaScript code
- ❌ Forms collecting sensitive data

### Email Authentication
Configure these before sending:

**SPF (Sender Policy Framework)**
```
v=spf1 include:mailgun.org ~all
```

**DKIM (DomainKeys Identified Mail)**
- Generated by your ESP
- Encrypts email authenticity

**DMARC (Domain-based Message Authentication, Reporting & Conformance)**
```
v=DMARC1; p=quarantine; rua=mailto:admin@arasllantas.com
```

---

## 📊 Performance Metrics

Track these key metrics:

| Metric | Good | Excellent |
|--------|------|-----------|
| Open Rate | 15%–25% | 25%–35% |
| Click Rate | 2%–5% | 5%–10% |
| Bounce Rate | <5% | <2% |
| Unsubscribe Rate | <0.5% | <0.2% |
| Complaint Rate | <0.1% | <0.05% |

### Improvement Tips
- Test subject lines A/B
- Optimize send time (test Tuesday-Thursday, 10am-2pm)
- Improve copy clarity
- Better CTA button placement
- Mobile optimization

---

## 🔄 Email Development Workflow

1. **Design in MJML** (`alt-moms-newsletter.mjml`)
2. **Compile to HTML** (`npm run build`)
3. **Preview locally** (open HTML in browser)
4. **Test in target clients** (Email on Acid, Litmus)
5. **Load test data** (use `data/sample-data.json`)
6. **Replace variables** (with backend)
7. **Re-test** (verify variables rendered)
8. **Send small sample** (QA recipients)
9. **Monitor** (track opens, clicks, bounces)

---

## 📚 Additional Resources

- **MJML Documentation**: https://mjml.io/documentation
- **Campaign Monitor Guides**: https://www.campaignmonitor.com/guides/
- **Email Standards**: https://www.litmus.com/resource-center
- **Accessibility**: https://www.litmus.com/email-accessibility/
- **Spam/Deliverability**: https://sendgrid.com/blog/email-deliverability/

---

**Last Updated**: February 25, 2026
