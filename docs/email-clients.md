# Email Client Compatibility Guide

Complete compatibility information for Ara's Llantas email templates across all major email clients.

## ✅ Fully Supported Clients

### Desktop Clients
| Client | Version | Status | Notes |
|--------|---------|--------|-------|
| **Gmail** | Latest | ✅ Full Support | Great rendering, full CSS support |
| **Outlook** | 2016+ | ✅ Full Support | Minor table quirks addressed in MJML |
| **Apple Mail** | Latest | ✅ Full Support | Excellent standards compliance |
| **Thunderbird** | Latest | ✅ Full Support | Standards-based rendering |

### Web-Based Clients
| Client | Status | Notes |
|--------|--------|-------|
| **Gmail Web** | ✅ Full Support | Excellent |
| **Outlook Web** | ✅ Full Support | Responsive design works well |
| **Yahoo Mail** | ✅ Full Support | Good CSS support |
| **AOL Mail** | ✅ Full Support | Similar to Yahoo |

### Mobile Clients
| App | Status | Notes |
|-----|--------|-------|
| **Gmail (iOS/Android)** | ✅ Full Support | Optimized rendering |
| **Apple Mail (iOS)** | ✅ Full Support | Excellent |
| **Outlook Mobile** | ✅ Full Support | Responsive design |
| **Samsung Mail** | ✅ Full Support | Good support |

---

## 🌙 Dark Mode Support

Modern email clients support dark mode. Our template includes proper dark mode detection:

```html
<meta name="color-scheme" content="light dark">
<meta name="supported-color-schemes" content="light dark">
```

### Dark Mode Rendering
- **Text**: Automatically inverted (white on black)
- **Backgrounds**: White becomes dark grey
- **Links**: Orange still visible
- **Images**: May need background color adjustment

**Testing dark mode:**
- Apple Mail: Preferences → View → Dark mode
- Gmail: Use system dark mode (macOS/iOS)
- Outlook: Settings → Display → Dark background

---

## ⚙️ Technical Compatibility

### CSS Support

**Fully Supported:**
- ✅ Inline styles
- ✅ `<style>` tags in `<head>`
- ✅ Media queries (for responsive design)
- ✅ Table-based layouts
- ✅ Basic gradients (some clients)

**Limited/Not Supported:**
- ❌ CSS Grid
- ❌ CSS Flexbox (use table layout instead)
- ❌ CSS animations
- ❌ CSS variables
- ❌ External stylesheets (won't load)

### HTML Support

**Supported Tags:**
- ✅ Tables, divs, spans
- ✅ Links (`<a>` with `href`, `target`, `rel`)
- ✅ Images (`<img>` with `src`, `alt`)
- ✅ Form elements (limited)
- ✅ Conditional comments `<!--[if mso]><!-->`

**Not Supported:**
- ❌ JavaScript (all clients block it)
- ❌ iframes
- ❌ Embedded video (use poster image + link)
- ❌ SVG (limited support)
- ❌ Forms that submit to endpoints

---

## 📊 Client-Specific Quirks & Fixes

### Outlook 2016–2019 (Desktop)
**Issue**: Doesn't support media queries
**Solution**: MJML handles this automatically with conditional comments

**Issue**: Doesn't support `border-radius`
**Solution**: We include VML fallback for rounded buttons

**Issue**: Extra padding on some elements
**Solution**: Use `mso-padding-alt` in styles

### Gmail
**Issue**: Gmail sometimes adds its own margins/padding
**Solution**: Use `padding` attributes to override

**Issue**: Gmail blocks some CSS in `<head>` styles
**Solution**: Always use inline styles as fallback

### Apple Mail / iOS
**Issue**: May add blue underline to dates/numbers
**Solution**: Wrap in `<span>` and style explicitly

**Issue**: Link colors may not render correctly
**Solution**: Always include `style="color:#FF6600;"`

### Yahoo/AOL
**Issue**: Limited CSS support
**Solution**: Stick to basic inline styles and table layouts

**Issue**: May strip some attributes
**Solution**: Redundantly specify in multiple ways

---

## 🎯 Recommended Testing Strategy

### Phase 1: Desktop Testing
1. **Gmail** (web + desktop app)
2. **Outlook** 2016+ (most problematic)
3. **Apple Mail** (reference for standards)

### Phase 2: Web Client Testing
1. **Outlook Web** (Office 365)
2. **Yahoo Mail**
3. **AOL Mail**

### Phase 3: Mobile Testing
1. **Gmail (Android)**
2. **Gmail (iOS)**
3. **Apple Mail (iOS)**
4. **Outlook Mobile**

### Phase 4: Automated Testing Tools
Use these for comprehensive testing:
- **Email on Acid**: https://www.emailonacid.com
- **Litmus**: https://www.litmus.com
- **MJML Email Preview**: Built into MJML CLI

---

## 🖥️ Rendering Differences by Client

### Button Background Color
| Client | Rendering | Issue |
|--------|-----------|-------|
| Most clients | Orange (#FF6600) | ✅ Perfect |
| Outlook 2016 | Orange (VML fallback) | ✅ Handled |
| Some mobile | May render slightly different | Acceptable |

**Our solution**: MJML generates both HTML5 and VML versions

### Line Height
| Client | Default | Our Setting |
|--------|---------|-------------|
| Gmail | 1.2 | 1.6 ✅ (more readable) |
| Outlook | 1.15 | 1.6 ✅ |
| Apple Mail | 1.6 | 1.6 ✅ |

### Font Support
| Font | Support | Fallback |
|------|---------|----------|
| Arial | ✅ Universal | Safe |
| Helvetica | ✅ Universal | Safe |
| System fonts | ❌ Risky | Avoid |
| Web fonts | ❌ No support | Use Arial/Helvetica |

**Our stack**: `font-family: Arial, Helvetica, sans-serif;`

---

## 📱 Mobile Optimization

### Viewport Meta Tag
```html
<meta name="viewport" content="width=device-width, initial-scale=1">
```

### Responsive Breakpoint
```mjml
<mj-breakpoint width="600px" />
```

### Mobile-First Approach
- Design for 320px width first
- Use `mj-column width="50%"` for 2-column layouts (stack on mobile)
- Test at: 320px, 480px, 600px widths

### Touch-Friendly Elements
- Buttons: Minimum 44×44px clickable area ✅
- Links: Adequate padding around them ✅
- Margins: 8px minimum between tappable elements ✅

---

## 🧪 Testing Checklists

### Pre-Send Checklist

**Desktop:**
- [ ] Gmail (web)
- [ ] Gmail (desktop app)  
- [ ] Outlook 2016+
- [ ] Apple Mail (macOS)
- [ ] Thunderbird

**Web:**
- [ ] Outlook.com
- [ ] Yahoo Mail
- [ ] AOL Mail

**Mobile:**
- [ ] Gmail (iOS app)
- [ ] Gmail (Android app)
- [ ] Apple Mail (iOS)
- [ ] Outlook (Mobile)

**Dark Mode:**
- [ ] macOS system dark mode
- [ ] iOS dark mode
- [ ] Gmail dark mode (if available)

### What to Check in Each Client

1. **Layout**
   - [ ] All sections visible
   - [ ] No overlapping text
   - [ ] Proper alignment

2. **Images**
   - [ ] All images load
   - [ ] Alt text displays when images blocked
   - [ ] Images correctly sized

3. **Text**
   - [ ] Font sizes readable
   - [ ] Line height not cramped
   - [ ] Colors have sufficient contrast

4. **Components**
   - [ ] Buttons clickable and properly styled
   - [ ] Links are underlined and colored correctly
   - [ ] Dividers visible

5. **Mobile**
   - [ ] Content fits 100% width
   - [ ] Single column (no side scrolling)
   - [ ] Buttons large enough to tap

---

## 🐛 Common Issues & Fixes

### Images Not Loading

**Cause**: Using non-HTTPS URLs or image too large
**Fix**:
- Use HTTPS-only URLs
- Compress images to <150KB
- Ensure CDN is accessible

**Test**: Add fallback text in email for when images are blocked

### Text Rendering Too Small/Large

**Cause**: Client overriding font size
**Fix**: Specify size in multiple ways:
```html
<div style="font-size:16px;" class="text-body">Your text</div>
```

### Buttons Not Clickable

**Cause**: Using `<div>` instead of `<a>` tags
**Fix**: Always use proper `<a>` tags or MJML `<mj-button>`

### Links Showing Ugly Blue Color

**Cause**: Client ignoring inline `color` style
**Fix**:
```html
<a href="url" style="color:#FF6600; text-decoration:none;">Link</a>
```

### Extra Spacing/Padding

**Cause**: Client adding default margins
**Fix**:
```html
<div style="margin:0; padding:8px;" mso-margin-alt="0">Content</div>
```

### Rounded Corners Not Showing in Outlook

**Cause**: Outlook 2016 doesn't support `border-radius`
**Fix**: MJML automatically adds VML fallback

---

## 📈 Deliverability Tips by Client

### Gmail
- Good deliverability if authenticated (SPF, DKIM, DMARC)
- Uses Gmail promotions tab for many emails
- Monitor click-to-open rate

### Outlook
- Stricter spam filtering
- Ensure proper authentication
- Avoid all-caps in subject lines

### Apple Mail
- Very lenient with content
- Best open/click rates typically
- Generally safe for sending

### Mobile Clients
- Usually follow web client rules
- Monitor mobile-specific bounces
- Test unsubscribe on mobile

---

## 🔍 Tools for Testing

| Tool | Purpose | Cost |
|------|---------|------|
| **Email on Acid** | Visual previews across clients | $$$$ |
| **Litmus** | Full testing + analytics | $$$ |
| **Who Can Use** | Accessibility check | Free |
| **MJML Preview** | Built-in MJML testing | Free |
| **Live Email Clients** | Real device testing | $$-$$$ |

**Minimum viable testing**: Gmail, Outlook, Apple Mail, iOS, Android

---

## 📚 Reference Links

- **Campaign Monitor Client Support**: https://www.campaignmonitor.com/css/
- **Email Standards Project**: https://www.emailstandards.org/
- **Litmus Guides**: https://www.litmus.com/resource-center
- **MJML Documentation**: https://mjml.io/documentation

---

**Last Updated**: February 25, 2026
