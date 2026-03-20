---
title: Print-Friendly Style Sheets Accessibility Best Practices
---

# Print-Friendly Style Sheets Accessibility Best Practices

This document defines best practices for implementing print-friendly CSS that produces accessible, well-structured printed documents from web pages. Print styles benefit all users — including those who rely on printed material for offline reading, reference, or assistive purposes.

Printing is an accessibility feature. Users with cognitive disabilities may find printed material easier to process; users with low vision may enlarge printed output; users without reliable internet access may need offline copies. A well-designed print stylesheet ensures that the content remains useful and readable in all contexts.

---

## 1. Core Principles

All print styles must:

- **Preserve content hierarchy**: Headings, lists, and document structure must be maintained in print output.
- **Maintain readable contrast**: Text must remain legible on white paper; avoid low-contrast color combinations.
- **Show important link destinations**: URLs should appear inline after link text so that content is usable without interactivity.
- **Suppress decorative and navigational content**: Navigation, advertisements, cookie banners, and other non-content elements should be hidden.
- **Use appropriate print units**: Prefer `pt`, `cm`, `mm`, or `in` units for print layouts; avoid `px` and `em` for physical dimensions.
- **Support page breaks logically**: Prevent headings and figure captions from being orphaned at the bottom of a page.

---

## 2. The `@media print` Media Query

All print-specific styles should be scoped within a `@media print` block or a separate stylesheet loaded with `media="print"`.

### Inline `@media print` block (recommended)

```css
@media print {
  /* All print styles here */
}
```

### Separate print stylesheet (alternative)

```html
<link rel="stylesheet" href="print.css" media="print">
```

Prefer the inline approach to keep styles co-located and reduce HTTP requests.

---

## 3. Hide Non-Essential Elements

Navigation, sidebars, footers, cookie notices, adverts, and interactive widgets serve no purpose in print. Hide them with `display: none`.

```css
@media print {
  nav,
  header nav,
  footer,
  aside,
  .sidebar,
  .cookie-banner,
  .skip-link,
  .ad,
  .social-share,
  .print-hide,
  video,
  audio,
  iframe,
  form button[type="submit"]:not(.print-show) {
    display: none !important;
  }
}
```

Use utility classes to control print visibility:

- `.print-hide` — added to elements that should never appear in print output (navigation buttons, interactive controls).
- `.print-show` — added to elements that are hidden on screen but should appear in print (e.g., expanded content, supplementary notes).

---

## 4. Reveal Link URLs

Sighted users who read a printed page cannot follow hyperlinks. Reveal the URL by inserting the `href` attribute value after link text using `::after`.

```css
@media print {
  a[href]::after {
    content: " (" attr(href) ")";
    font-size: 0.875em;
    color: #333;
    word-break: break-all;
  }

  /* Suppress URL display for non-informative links */
  a[href^="#"]::after,
  a[href^="javascript:"]::after,
  a[href].no-print-url::after {
    content: "";
  }
}
```

> **Note:** Always suppress URLs for fragment (`#`) links and JavaScript pseudo-links, as they are not meaningful in print.

---

## 5. Typography for Print

Screen typography optimised for on-screen readability may not transfer well to print. Adjust for physical media:

```css
@media print {
  body {
    font-family: Georgia, "Times New Roman", Times, serif;
    font-size: 12pt;
    line-height: 1.5;
    color: #000;
    background: #fff;
  }

  h1 { font-size: 22pt; }
  h2 { font-size: 18pt; }
  h3 { font-size: 14pt; }

  p, li, td, th {
    font-size: 11pt;
    orphans: 3;
    widows: 3;
  }

  blockquote {
    border-left: 3pt solid #555;
    padding-left: 12pt;
    margin-left: 0;
    font-style: italic;
  }
}
```

- Use serif typefaces for body text — they are generally more legible in print.
- Set `orphans` and `widows` to prevent isolated lines at the top or bottom of a page.
- Use `pt` for font sizes in print stylesheets.

---

## 6. Page Setup and Margins

Define page margins and orientation using the `@page` rule:

```css
@page {
  margin: 2cm;
  size: A4 portrait; /* or letter, A4 landscape, etc. */
}

@page :first {
  margin-top: 3cm; /* Extra top margin on the first page */
}

@page :left {
  margin-left: 3cm;
}

@page :right {
  margin-right: 3cm;
}
```

Generous margins improve readability and allow for binding or annotation.

---

## 7. Page Break Control

Avoid breaking content mid-heading, mid-figure, or mid-table by using page break properties:

```css
@media print {
  h1, h2, h3 {
    page-break-after: avoid;
    break-after: avoid;
  }

  figure, img, table, pre, blockquote {
    page-break-inside: avoid;
    break-inside: avoid;
  }

  /* Force a page break before major sections */
  .page-break-before {
    page-break-before: always;
    break-before: always;
  }

  /* Keep a heading with the paragraph that follows */
  h2, h3 {
    page-break-after: avoid;
    break-after: avoid;
  }
}
```

Use both `page-break-*` (legacy) and `break-*` (modern) properties for broad browser support.

---

## 8. Images and Media

Images should scale to fit the page without overflowing:

```css
@media print {
  img,
  svg {
    max-width: 100% !important;
    height: auto;
    page-break-inside: avoid;
    break-inside: avoid;
  }

  /* Hide purely decorative background images */
  * {
    background-image: none !important;
  }
}
```

> **Accessibility note:** Browsers typically suppress CSS background images in print output by default. Ensure that meaningful images use `<img>` elements with descriptive `alt` text rather than CSS backgrounds.

---

## 9. Tables

Tables should remain readable across page boundaries:

```css
@media print {
  table {
    border-collapse: collapse;
    max-width: 100%;
  }

  th, td {
    border: 1pt solid #333;
    padding: 6pt;
    text-align: left;
  }

  thead {
    display: table-header-group; /* Repeat table header on every page */
  }

  tr {
    page-break-inside: avoid;
    break-inside: avoid;
  }
}
```

`display: table-header-group` on `<thead>` ensures column headings are repeated when a table spans multiple pages, which is critical for users who need to refer back to column labels.

---

## 10. Color and Contrast

Do not rely on color alone to convey meaning in print — ink color rendering can vary significantly between printers:

```css
@media print {
  /* Remove color-coded highlights; use borders instead */
  .status-warning {
    border: 2pt solid #000;
    color: #000;
    background: none;
  }

  /* Ensure sufficient contrast for grayscale printing */
  .label-primary {
    color: #000;
    background: #fff;
    border: 1pt solid #000;
  }
}
```

Always test print output in grayscale mode to ensure content remains distinguishable without color.

### Grayscale testing checklist

- [ ] All text is legible in black-and-white output
- [ ] Charts and graphs include patterns, shapes, or labels in addition to color
- [ ] Status indicators (warnings, errors, successes) use text labels, not color alone
- [ ] Links remain distinguishable (underline or URL disclosure)

---

## 11. Providing a Print Button

Offering a dedicated "Print this page" button improves discoverability and helps users who are not familiar with browser print shortcuts.

```html
<button type="button" class="print-trigger print-hide" onclick="window.print()">
  <svg aria-hidden="true" focusable="false" width="16" height="16">
    <use href="#icon-printer"></use>
  </svg>
  Print this page
</button>
```

```css
@media print {
  .print-hide {
    display: none !important;
  }
}
```

- Use `type="button"` to prevent accidental form submission.
- Include visible button text (not icon-only) for screen reader users.
- Hide the button in print output itself — it serves no purpose once printed.

---

## 12. Print-Specific Header and Footer Content

Use `@page` margins to add running headers and footers (where supported):

```css
@page {
  @top-center {
    content: "Document Title — " string(doc-title);
    font-size: 9pt;
    color: #555;
  }

  @bottom-right {
    content: "Page " counter(page) " of " counter(pages);
    font-size: 9pt;
    color: #555;
  }
}
```

> **Browser support note:** The `@page` margin boxes (`@top-center`, `@bottom-right`, etc.) are primarily supported in paged media renderers (e.g., Prince, WeasyPrint, Paged.js) and not all browsers. For cross-browser page numbers, use JavaScript or server-side PDF generation.

---

## 13. Progressive Enhancement for Print

Print styles should be a progressive enhancement. The base content must remain accessible even if print CSS is not applied:

1. **Start with semantic HTML**: Correct heading hierarchy, lists, and landmark regions ensure the document is readable even without styles.
2. **Screen styles first**: Write screen CSS, then layer print styles inside `@media print`.
3. **Test without print CSS**: Confirm the content is still comprehensible in the browser's default print mode before adding custom styles.

---

## 14. Libraries and Tools

Several libraries and bookmarklets can assist with print-friendly CSS:

- **[print-friendly](https://github.com/hyunbinseo/print-friendly)** — A lightweight utility for identifying and suppressing non-printable elements.
- **[Paged.js](https://pagedjs.org/)** — An open-source library for rendering paged content in the browser, supporting CSS Paged Media Level 3 specifications including `@page` margin boxes.
- **[WeasyPrint](https://weasyprint.org/)** — A Python library that converts HTML/CSS to PDF with full support for CSS Paged Media.
- **[Prince XML / PrintedCSS](https://printedcss.com/)** — A commercial HTML-to-PDF engine with comprehensive paged media CSS support.
- **[print-css-bookmarklet](https://github.com/mgifford/print-css-bookmarklet)** — A bookmarklet for testing print styles in real-time in the browser without opening the print dialog.

---

## 15. Testing Print Styles

### Browser testing

1. **Chrome/Edge**: Open DevTools → More tools → Rendering → Set "Emulate CSS media type" to `print`. The page immediately re-renders with print styles applied.
2. **Firefox**: Open DevTools → Inspector → toggle the print media icon in the toolbar.
3. **Print Preview**: Use `Ctrl+P` / `Cmd+P` to open the native print dialog and preview across paper sizes and orientations.

### Accessibility testing for print output

- **Check heading structure**: Confirm that headings in print output follow a logical hierarchy.
- **Verify link URLs**: Confirm that all meaningful links display their destination URL.
- **Test in grayscale**: Print or preview in black and white to verify color-independent readability.
- **Check image alt text**: Confirm that images have descriptive `alt` attributes (browsers may display alt text for broken images in print).
- **Validate page breaks**: Ensure no headings appear at the bottom of a page without the following content.

### Automated testing

- **axe-core**: Run standard accessibility checks on the print-rendered DOM by activating print media emulation in DevTools before running axe.
- **Lighthouse**: Does not directly test print styles, but accessibility violations in the screen version also affect print output.

---

## 16. WCAG 2.2 Criteria Relevant to Print

Although WCAG 2.2 applies to web content, the following success criteria are relevant to ensuring printed output is accessible:

| SC | Title | Relevance to Print |
| :--- | :--- | :--- |
| 1.1.1 | Non-text Content | Images must have `alt` text to remain meaningful when background images are suppressed |
| 1.3.1 | Info and Relationships | Structural relationships (headings, lists, tables) must be preserved in print output |
| 1.3.2 | Meaningful Sequence | Reading order must be logical when print layout differs from screen layout |
| 1.4.1 | Use of Color | Color must not be the sole means of conveying information; critical in grayscale printing |
| 1.4.3 | Contrast (Minimum) | Text must maintain 4.5:1 contrast ratio on the white/light paper background |
| 1.4.6 | Contrast (Enhanced) | Target 7:1 for printed documents where fine text is common |
| 2.4.4 | Link Purpose (In Context) | Disclosed URLs must be sufficient to understand the link destination in print |

---

## References

### W3C Specifications

- [CSS Paged Media Module Level 3](https://www.w3.org/TR/css-page-3/) — Defines `@page` rules, page breaks, and margin boxes
- [CSS Media Queries Level 5](https://www.w3.org/TR/mediaqueries-5/) — Defines `@media print` and related media types
- [CSS Generated Content for Paged Media](https://www.w3.org/TR/css-gcpm-3/) — Running headers, footnotes, and cross-references
- [CSS Fragmentation Module Level 3](https://www.w3.org/TR/css-break-3/) — Defines `break-before`, `break-after`, `break-inside`, `orphans`, `widows`
- [MDN: Printing — CSS Media Queries Guide](https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Media_queries/Printing)

### Machine-Readable Standards

For AI systems and automated tooling, see [wai-yaml-ld](https://github.com/mgifford/wai-yaml-ld) for structured accessibility standards:

- [WCAG 2.2 (YAML)](https://github.com/mgifford/wai-yaml-ld/blob/main/kitty-specs/001-wai-standards-yaml-ld-ingestion/research/wcag-2.2-normative.yaml) — Machine-readable WCAG 2.2 normative content
- [CSS Specifications Index (YAML)](https://github.com/mgifford/wai-yaml-ld/blob/main/kitty-specs/001-wai-standards-yaml-ld-ingestion/research/css-specifications-index.yaml) — CSS specs including paged media

### Related Guides

- [Light/Dark Mode Accessibility Best Practices](./LIGHT_DARK_MODE_ACCESSIBILITY_BEST_PRACTICES.md) — CSS media queries and color contrast management
- [User Personalization Accessibility Best Practices](./USER_PERSONALIZATION_ACCESSIBILITY_BEST_PRACTICES.md) — CSS preference media queries
- [Progressive Enhancement Best Practices](./PROGRESSIVE_ENHANCEMENT_BEST_PRACTICES.md) — Building accessible layered experiences

### Additional Reading

- [Designing For Print With CSS — Smashing Magazine](https://www.smashingmagazine.com/2015/01/designing-for-print-with-css/)
- [CSS Printer-Friendly Pages — SitePoint](https://www.sitepoint.com/css-printer-friendly-pages/)
- [Print Your Way — A List Apart](https://alistapart.com/article/printyourway/)
- [Printing the Web: Making Webpages Look Good on Paper — Piccalilli](https://piccalil.li/blog/printing-the-web-making-webpages-look-good-on-paper/)
- [Make Your Website Printable With CSS](https://barish.me/blog/make-your-website-printable-with-css/)
- [Small Advice for Writing CSS for Print](https://dev.to/vlasterx/small-advice-for-writing-the-css-for-the-print-4mc7)
- [Easy Website Test for Print-Friendly Design in Any Browser — Rocket Solid](https://www.rocketsolid.ca/blog/Easy-Website-Test-for-Print-Friendly-Design-in-Any-Browser)
- [The Surprisingly Exciting World of Print/PDF CSS — Syntax.fm Episode 711](https://syntax.fm/show/711/the-surprisingly-exciting-world-of-print-pdf-css/transcript)

---

AGPL-3.0-or-later License - See LICENSE file for full text  
Copyright (c) 2026 Mike Gifford
