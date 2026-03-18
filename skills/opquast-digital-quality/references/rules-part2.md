# Opquast Rules 173-244: Detailed Reference

## Table of Contents

- [10. Newsletter (Rules 173-179)](#10-newsletter-rules-173-179)
- [11. Presentation (Rules 180-196)](#11-presentation-rules-180-196)
- [12. Security (Rules 197-217)](#12-security-rules-197-217)
- [13. Server and Performance (Rules 218-230)](#13-server-and-performance-rules-218-230)
- [14. Structure and Code (Rules 231-244)](#14-structure-and-code-rules-231-244)

---

## 10. Newsletter (Rules 173-179)

- Use confirmed opt-in — send confirmation email before activating subscription (rule 173).
- Include unsubscribe link in every newsletter (rule 174).
- Unsubscribing from newsletter link must not require separate email confirmation (rule 175).
- Users must also be able to unsubscribe from the website (rule 176).
- Make latest newsletter edition available online (rule 177).
- Maintain newsletter archives online (rule 178).
- State newsletter frequency before subscription (rule 179).

---

## 11. Presentation (Rules 180-196)

### 11.1 Visual Consistency and Colour

- Maintain consistent visual identity across all pages (rule 180).
- Do not convey information by colour alone — provide secondary indicator (icon, text label) (rule 181).
- Ensure sufficient contrast: minimum 4.5:1 for normal text, 3:1 for large text (WCAG 2.2 AA) (rule 182).

```css
body {
  color: #1a1a1a;      /* ~16:1 contrast on white */
  background: #ffffff;
}
```

### 11.2 Styles and Meaning

- Content must remain understandable when styles are disabled (rule 183).
- Never refer to content solely by shape or position — "Click the button on the right" (rule 184).
- Do not hide content from screen readers that is needed to understand the page (rule 185).
- Touch/click targets must be at least 24x24 CSS pixels; aim for 44x44 for primary controls (rule 186).
- Do not replace styleable text with images of text (rule 187).
- If CSS-generated content (`::before`, `::after`) carries meaning, provide text alternative (rule 188).
- Typographic symbols used decoratively must have text alternatives or be hidden from screen readers (rule 189).
- Font family declarations must end with generic family fallback (rule 190).

```css
body {
  font-family: "Roboto", "Helvetica Neue", Arial, sans-serif;
}
```

- Do not use `text-align: justify` for body text (rule 191).
- Use CSS `text-transform` for decorative capitalisation, not uppercase HTML text (rule 192).

### 11.3 Responsiveness and Print

- Do not block browser zoom — remove `maximum-scale=1` and `user-scalable=no` (rule 193).

```html
<!-- Wrong -->
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no" />
<!-- Right -->
<meta name="viewport" content="width=device-width, initial-scale=1" />
```

- Provide at least one mechanism for adapting layout to mobile — responsive CSS (rule 194).
- Provide print styles — hide navigation, ensure readable body text (rule 195).
- Page content must be printable without navigation blocks (rule 196).

```css
@media print {
  nav, header, footer, .sidebar, .no-print { display: none; }
  body { font-size: 12pt; color: #000; }
  a[href]::after { content: " (" attr(href) ")"; }
}
```

---

## 12. Security (Rules 197-217)

### 12.1 HTTPS and Transport Security

- All pages must use HTTPS; HTTP must redirect to HTTPS (rule 197).
- TLS certificates must be signed by trusted authority and currently valid (rule 198).
- Send HSTS header on HTTPS pages (rule 199).
- HTTPS pages must not load HTTP resources — no mixed content (rule 200).

```http
Strict-Transport-Security: max-age=31536000; includeSubDomains; preload
```

### 12.2 Password Management

- Users must complete all password operations online (rule 201).
- Allow users to choose and change passwords freely (rule 202).
- Implement password-strength indicator (rule 203).
- Provide self-service password reset (rule 204).
- Never transmit passwords in plain text — not in emails, logs, or URLs (rule 205).

### 12.3 HTTP Security Headers

- Send `X-Content-Type-Options: nosniff` (rule 206).
- Every response must include explicit `Content-Type` header (rule 207).
- Display transaction security information when applicable (rule 208).
- Disable directory listing on all web servers (rule 209).
- Send `X-XSS-Protection: 1; mode=block` for older browser defence (rule 210).
- Send `X-Frame-Options` or CSP `frame-ancestors` directive (rule 211).

```http
X-Frame-Options: SAMEORIGIN
Content-Security-Policy: frame-ancestors 'self';
```

- Implement `Content-Security-Policy` header (rule 212).

```http
Content-Security-Policy: default-src 'self'; script-src 'self' https://trusted-cdn.example.com; style-src 'self' 'unsafe-inline';
```

- Do not expose server software versions — remove `Server` and `X-Powered-By` headers (rule 213).
- Apply Subresource Integrity (`integrity`) to third-party scripts and stylesheets (rule 214).

```html
<script src="https://cdn.example.com/library.min.js"
  integrity="sha384-<hash>" crossorigin="anonymous"></script>
```

- Require at least two confirmation factors for sensitive operations (rule 215).
- Never suppress or replace the browser address bar (rule 216).
- Authenticate email domain using SPF, DKIM, and DMARC records (rule 217).

---

## 13. Server and Performance (Rules 218-230)

### 13.1 URLs and Redirects

- Site must work with and without `www` prefix — redirect one to the other (rule 218).
- Web root must contain `robots.txt` (rule 219).
- Provide `sitemap.xml` listing crawlable content (rule 220).
- Do not force redirects from desktop to mobile version or app (rule 221).
- Return proper `404 Not Found` HTTP status for missing pages (rule 222).

### 13.2 Error Pages

- Provide custom 404 error page with helpful message (rule 223).
- Include main navigation menu on custom error pages (rule 225).

### 13.3 Compression, Caching, and Minification

- Enable HTTP compression (gzip or Brotli) for text-based resources (rule 226).
- Send cache-control headers for static assets (rule 227).

```http
Cache-Control: public, max-age=31536000, immutable
```

- Include character set in `Content-Type` response headers (rule 228).

```http
Content-Type: text/html; charset=utf-8
```

- Minify CSS files before deployment (rule 229).
- Minify JavaScript files before deployment (rule 230).

---

## 14. Structure and Code (Rules 231-244)

### 14.1 Metadata and Encoding

- Make publication and update dates machine-readable — use `<time datetime="...">` or structured data (rule 231).

```html
<time datetime="2025-03-01">1 March 2025</time>
```

- Each page `<head>` must declare character set (rule 232).
- Use UTF-8 encoding consistently (rule 233).

```html
<meta charset="UTF-8" />
```

### 14.2 Headings and Semantic Structure

- Organise content using hierarchical heading structure `h1` > `h2` > `h3` — do not skip levels (rule 234).
- Mark up displayed lists as `<ul>`, `<ol>`, or `<dl>` (rule 235).
- HTML `id` attributes must be unique within each page (rule 236).

### 14.3 User Control of Content

- Do not block copy/paste — do not use `user-select: none` on body text (rule 237).
- Do not block browser context menu (rule 238).
- Do not use client-side automatic redirection or `<meta http-equiv="refresh">` — use server-side 301/302 (rule 239).

### 14.4 Documents and Tables

- Internal PDF documents must have selectable text — use tagged, not scanned, PDFs (rule 240).
- Structure internal PDFs with headings (rule 241).
- Associate data table cells with headers using `<th scope="...">` or `headers` attribute (rule 242).
- Provide captions for data tables using `<caption>` (rule 243).
- Layout tables must linearise without loss of meaning — prefer CSS layout (rule 244).

```html
<table>
  <caption>Quarterly revenue by region (USD thousands)</caption>
  <thead>
    <tr>
      <th scope="col">Region</th>
      <th scope="col">Q1</th>
      <th scope="col">Q2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">North America</th>
      <td>1,200</td>
      <td>1,450</td>
    </tr>
  </tbody>
</table>
```
