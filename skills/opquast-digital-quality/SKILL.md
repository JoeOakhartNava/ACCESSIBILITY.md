---
name: opquast-digital-quality
description: Apply the Opquast Digital Quality Framework (245 rules, 14 categories) when building, reviewing, or auditing websites and web applications. Use for web development quality assurance, accessibility compliance, security hardening, privacy implementation, e-commerce best practices, and holistic digital quality checks.
---

# Opquast Digital Quality Best Practices

Apply the [Opquast Digital Quality Checklist](https://checklists.opquast.com/en/digital-quality/) (Version 5, 2025-2030) when building or reviewing web projects. The framework contains 245 rules across 14 categories covering content, privacy, e-commerce, forms, accessibility, security, performance, and more.

> Rules are published under [Creative Commons BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/). See [opquast.com](https://www.opquast.com/) for the authoritative source.

## When to Use This Skill

- Building a new website or web application
- Reviewing or auditing an existing site for quality
- Implementing accessibility, security, or privacy features
- Creating e-commerce flows, forms, or navigation systems
- Generating HTML/CSS code that must meet quality standards

## Quick Reference: AI-Enforceable Requirements

| Category | Key Requirements |
|---|---|
| Content | Metadata (`description`, OG tags), alt text, explicit dates, abbreviation expansion, data tables for charts |
| Privacy | Privacy link in footer, generic auth-failure messages, HTTPS for sensitive data, no sensitive data in URLs |
| E-Commerce | No pre-checked opt-ins, availability before checkout, explicit pricing, two+ payment methods |
| Forms | Associated `<label>`, `aria-required`, `aria-invalid`, `aria-describedby`, correct `input type`, `autocomplete` |
| Identification | Unique page titles (`Page \| Site`), `lang` attribute, favicon, two+ contact methods |
| Images/Media | Meaningful `alt` text, empty `alt=""` for decorative, captions/transcripts for AV, no autoplay |
| Internationalisation | `lang` on `<html>`, `hreflang` for translations, international dialling codes |
| Links | Descriptive anchor text (no "click here"), `tel:` protocol, file type/size for downloads |
| Navigation | Skip links, visible `:focus-visible`, logical tab order, consistent nav placement, breadcrumbs |
| Presentation | Contrast 4.5:1 normal / 3:1 large text, no colour-only info, no blocked zoom, responsive, print styles |
| Security | HTTPS everywhere, HSTS, CSP, SRI, no plain-text passwords, security headers |
| Server/Performance | `robots.txt`, `sitemap.xml`, gzip/Brotli, cache headers, minified CSS/JS |
| Structure/Code | UTF-8, unique IDs, no `meta refresh`, tagged PDFs, heading hierarchy `h1>h2>h3` |
| Newsletter | Confirmed opt-in, unsubscribe link, archives online, state frequency |

## Workflow

### 1. Identify Applicable Categories

Not all 14 categories apply to every project. Select relevant ones:

- **All web projects**: Content, Forms, Identification, Images/Media, Links, Navigation, Presentation, Security, Server/Performance, Structure/Code
- **Add if applicable**: E-Commerce (online sales), Newsletter (email marketing), Internationalisation (multi-language), Privacy (user accounts/data collection)

### 2. Load Detailed Rules

Read the relevant reference file for the full rules with code examples:

- **Rules 1-172** (Content, Privacy, E-Commerce, Forms, Identification, Images/Media, Internationalisation, Links, Navigation): See `references/rules-part1.md`
- **Rules 173-244** (Newsletter, Presentation, Security, Server/Performance, Structure/Code): See `references/rules-part2.md`

### 3. Apply During Development

When generating or reviewing code, apply rules as implementation requirements:

- **HTML generation**: Include metadata, semantic structure, accessible forms, proper alt text
- **CSS generation**: Ensure contrast ratios, focus styles, responsive layout, print styles
- **Server configuration**: Set security headers, enable compression, configure caching
- **Content review**: Check date formats, link text, abbreviation expansion

### 4. Validate

After implementation, verify against the applicable rules. Common automated checks:

- HTML validation and heading hierarchy
- Colour contrast ratios (4.5:1 normal text, 3:1 large text)
- Missing alt attributes
- Missing form labels
- Security headers present
- Broken internal links

## Essential Code Patterns

### Page Template Minimum

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <meta name="description" content="Page description here." />
  <meta property="og:title" content="Page Title | Site Name" />
  <meta property="og:description" content="Page description for sharing." />
  <title>Page Title | Site Name</title>
  <link rel="icon" type="image/svg+xml" href="/favicon.svg" />
</head>
<body>
  <a href="#main" class="skip-link">Skip to main content</a>
  <nav aria-label="Main navigation"><!-- ... --></nav>
  <main id="main"><!-- ... --></main>
  <footer>
    <a href="/privacy">Privacy Policy</a>
    <a href="/terms">Terms of Use</a>
  </footer>
</body>
</html>
```

### Accessible Form Field

```html
<label for="email">
  Email address <span aria-hidden="true">*</span>
  <span class="hint" id="email-hint">Required. Example: user@example.com</span>
</label>
<input id="email" type="email" autocomplete="email"
  aria-required="true" aria-describedby="email-hint" />
```

### Security Headers (Server Config)

```
Strict-Transport-Security: max-age=31536000; includeSubDomains; preload
X-Content-Type-Options: nosniff
X-Frame-Options: SAMEORIGIN
Content-Security-Policy: default-src 'self';
Referrer-Policy: strict-origin-when-cross-origin
```

### Focus and Contrast Styles

```css
:focus-visible {
  outline: 3px solid #005fcc;
  outline-offset: 2px;
}
body {
  color: #1a1a1a; /* ~16:1 contrast on white */
  background: #ffffff;
}
```

### Print Styles

```css
@media print {
  nav, header, footer, .sidebar, .no-print { display: none; }
  body { font-size: 12pt; color: #000; }
  a[href]::after { content: " (" attr(href) ")"; }
}
```

## WCAG Relationship

Opquast complements WCAG 2.2 rather than replacing it. Key overlaps:

| Opquast Rules | WCAG Criteria |
|---|---|
| 116-118 (image alt) | 1.1.1 Non-text Content |
| 121-122 (transcripts) | 1.2.2, 1.2.3, 1.2.5 |
| 124-127 (no autoplay) | 1.4.2, 2.2.2 |
| 165-167 (keyboard/focus) | 2.1.1, 2.4.3 |
| 181 (not colour alone) | 1.4.1 |
| 182 (contrast) | 1.4.3, 1.4.6 |
| 193 (no zoom block) | 1.4.4 |

Use Opquast as a holistic quality baseline and layer WCAG testing for full accessibility compliance.
