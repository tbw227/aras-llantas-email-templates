# Changelog

All notable changes to Ara's Llantas email templates will be documented in this file.

## [1.0.0] - 2026-02-25

### Initial Release
- ✅ Spring lawn mower tire promotion template
- ✅ Black, orange, grey, and white color scheme
- ✅ Mobile-responsive design (tested at 320px–600px)
- ✅ Full email client compatibility (Gmail, Outlook, Apple Mail, Yahoo, AOL)
- ✅ Dark mode support
- ✅ MJML source with automatic HTML compilation
- ✅ Security best practices: `rel="noopener noreferrer"` on all external links
- ✅ CAN-SPAM compliance (unsubscribe link, business address, etc.)

### Added
- MJML source template (`alt-moms-newsletter.mjml`)
- Build system with npm scripts (`package.json`)
- Comprehensive documentation:
  - `README.md` - Full project guide
  - `QUICKSTART.md` - Get started in 5 minutes
  - `docs/best-practices.md` - Email development standards
  - `docs/email-clients.md` - Client compatibility reference
  - `docs/testing-guide.md` - Testing procedures
- Sample data files:
  - `data/sample-data.json` - Test data for template rendering
  - `data/variables-guide.md` - Template variable documentation
- Environment template (`.env.example`)
- CSS utilities (`css/styles.css`)
- `.gitignore` - Git configuration
- `.prettierrc` - Code formatting config

### Features
- Tested brand colors (black, orange, grey, white)
- Responsive for all screen sizes
- Optimized for mobile-first design
- Working contact links (phone, email)
- Track-ready links with UTM parameters
- Alt text on all images
- Proper font stacks (Arial, Helvetica fallback)
- Accessible color contrast ratios

### Template Variables
- `{{ webview_url }}` - View email in browser
- `{{ preferences_url }}` - Email preferences link
- `{{ unsubscribe_url }}` - Unsubscribe link (required)
- `{{ business_address_line_1 }}` - Street address
- `{{ business_city }}` - City name
- `{{ business_state }}` - State abbreviation
- `{{ business_zip }}` - ZIP code

---

## Planned Features (v1.1.0)

- [ ] Social media sharing buttons
- [ ] Customer testimonials section
- [ ] Product comparison table
- [ ] Dynamic pricing based on region
- [ ] A/B test variants (subject lines, CTA copy)
- [ ] Advanced analytics tracking
- [ ] Automated send-time optimization
- [ ] Multi-language support

---

## Version Format

This project follows [Semantic Versioning](https://semver.org/):

- **MAJOR** version when incompatible changes (breaking email structure)
- **MINOR** version when new features added
- **PATCH** version for bug fixes

## How to Update

### From Template Developers
1. Make changes to `alt-moms-newsletter.mjml`
2. Test thoroughly with `npm run watch`
3. Update this `CHANGELOG.md` with changes
4. Commit with descriptive message
5. Tag release: `git tag v1.0.1`

### From Users
1. Pull latest version: `git pull`
2. Reinstall dependencies: `npm install`
3. Run tests: `npm run test`
4. Review changes in this file
5. Update `.env` if needed for new variables

---

## Known Issues

None currently reported.

## Support

- 📖 See [README.md](README.md) for full documentation
- 🧪 See [docs/testing-guide.md](docs/testing-guide.md) for troubleshooting
- 📧 Contact: support@arasllantas.com

---

Last Updated: February 25, 2026
