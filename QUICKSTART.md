# Quick Start Guide

Get started with Ara's Llantas email templates in 5 minutes.

## ⚡ Setup (2 min)

### 1. Install Dependencies
```bash
npm install
```

### 2. Check Installation
```bash
npm run build
```

Should create/update `aras-llantas-spring-promo.html` without errors.

---

## 🚀 Development Workflow (5 min)

### Watch Mode (Auto-compile)
```bash
npm run watch
```

This watches `alt-moms-newsletter.mjml` and automatically compiles to HTML whenever you save.

### View Your Email
1. Open `aras-llantas-spring-promo.html` in your browser
2. Use DevTools to test mobile (Ctrl+Shift+I → Device Toolbar)
3. Test in multiple browsers/devices

---

## 📧 Before Sending

### 1. Replace Template Variables

Edit in your backend before sending (see `data/variables-guide.md`):

```javascript
// Node.js example
const html = fs.readFileSync('aras-llantas-spring-promo.html', 'utf-8')
  .replace('{{ business_address_line_1 }}', '4521 E Van Buren Street')
  .replace('{{ business_city }}', 'Phoenix')
  .replace('{{ business_state }}', 'AZ')
  .replace('{{ business_zip }}', '85008')
  .replace('{{ webview_url }}', 'https://arasllantas.com/email/view/abc')
  .replace('{{ preferences_url }}', 'https://arasllantas.com/prefs/xyz')
  .replace('{{ unsubscribe_url }}', 'https://arasllantas.com/unsub/123');
```

### 2. Send Test Email
Send to yourself and check in Gmail, Outlook, Apple Mail

### 3. Follow Testing Checklist
See `docs/testing-guide.md` for complete pre-send checklist

---

## 📚 Key Files & Folders

| File/Folder | Purpose |
|-------------|---------|
| `aras-llantas-spring-promo.mjml` | **Source template** - Edit this |
| `aras-llantas-spring-promo.html` | **Compiled output** - This gets sent |
| `package.json` | Build scripts and dependencies |
| `.env.example` | Template variables reference |
| `data/` | Sample data and variable docs |
| `docs/` | Complete guides (best practices, testing, etc.) |
| `images/` | Email images and assets |
| `css/` | Email-safe CSS utilities |

---

## 🎨 Customizing the Template

### Change Colors
Edit `alt-moms-newsletter.mjml`:
```mjml
<mj-button background-color="#FF6600">  <!-- Change here -->
  Shop Now
</mj-button>
```

### Change Content
Edit text between `<mj-text>` tags:
```mjml
<mj-text font-size="22px" font-weight="700">
  Edit this headline
</mj-text>
```

### Add/Remove Sections
Copy entire `<mj-section>` blocks or delete them.

---

## 📖 Documentation

- **Complete Index**: See [README.md](README.md)
- **Email Standards**: See [docs/best-practices.md](docs/best-practices.md)
- **Testing Procedures**: See [docs/testing-guide.md](docs/testing-guide.md)
- **Email Client Issues**: See [docs/email-clients.md](docs/email-clients.md)
- **Template Variables**: See [data/variables-guide.md](data/variables-guide.md)

---

## 🆘 Troubleshooting

### MJML not compiling?
```bash
npm install mjml
npm run build
```

### Images not showing?
- Use absolute URLs: `https://cdn.example.com/image.jpg` (not `./images/file.jpg`)
- Use HTTPS only
- Keep images under 150KB

### Variables showing as `{{ variable }}`?
- You need to process template in backend
- See `data/variables-guide.md` for examples
- Don't send without replacing variables

### Buttons not showing color?
- Check browser cache (Ctrl+Shift+Delete)
- Rebuild: `npm run build`
- Test in different email client

---

## 📞 Quick Reference

```bash
# Compile once
npm run build

# Watch for changes (auto-compile)
npm run watch

# Test email content
npm run test
```

---

## ✅ Next Steps

1. ✅ Install dependencies: `npm install`
2. ✅ Edit `alt-moms-newsletter.mjml` with your content
3. ✅ Run: `npm run watch`
4. ✅ Preview: Open `alt-moms-newsletter.html` in browser
5. ✅ Test mobile: Use DevTools responsive mode
6. ✅ Replace variables: See `data/variables-guide.md`
7. ✅ Pre-send checklist: See `docs/testing-guide.md`
8. ✅ Send!

---

**Need help?** See the full [README.md](README.md) for more details.
