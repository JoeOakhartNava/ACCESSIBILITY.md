# Opquast Rules 1-172: Detailed Reference

## Table of Contents

- [1. Content (Rules 1-14)](#1-content-rules-1-14)
- [2. Personal Data and Privacy (Rules 15-29)](#2-personal-data-and-privacy-rules-15-29)
- [3. E-Commerce (Rules 30-68)](#3-e-commerce-rules-30-68)
- [4. Forms (Rules 69-98)](#4-forms-rules-69-98)
- [5. Identification and Contact (Rules 99-115)](#5-identification-and-contact-rules-99-115)
- [6. Images and Media (Rules 116-127)](#6-images-and-media-rules-116-127)
- [7. Internationalisation (Rules 128-135)](#7-internationalisation-rules-128-135)
- [8. Links (Rules 136-152)](#8-links-rules-136-152)
- [9. Navigation (Rules 153-172)](#9-navigation-rules-153-172)

---

## 1. Content (Rules 1-14)

### 1.1 Content Discovery and Metadata

- Provide a mechanism to discover new content or services — "What's new" page, RSS/Atom feed, or newsletter (rule 1).
- Include clear copyright and reuse rights information accessible from every page (rule 2).
- Each page `<head>` must contain `<meta name="description">` and Open Graph tags (rule 3).

```html
<head>
  <meta name="description" content="A concise description of this page's content." />
  <meta property="og:title" content="Page Title" />
  <meta property="og:description" content="Page description for social sharing." />
</head>
```

### 1.2 Dates, Vocabulary, and Transparency

- Present dates in explicit, unambiguous format — "15 March 2025" not "15/03/25" (rule 4).
- Expand abbreviations and acronyms on first use. Use `<abbr title="...">` in HTML (rule 5).
- Indicate publication or last-updated date when content timeliness matters (rule 6).
- Provide a lexicon or glossary for technical vocabulary (rule 7).

### 1.3 Advertising, Moderation, and Data Visualisation

- Label advertisements and sponsored content visually and programmatically (rule 8).
- Publish moderation rules for user-generated public spaces (rule 9).
- Allow users to preview uploaded files before submission (rule 10).
- Provide at least one mechanism to report abuse in public spaces (rule 11).
- Accompany every chart/graph with underlying numerical data (rule 12).
- Search results must display total results, current page, results per page (rule 13).
- Do not use special characters to simulate visual formatting not conveyed semantically (rule 14).

---

## 2. Personal Data and Privacy (Rules 15-29)

### 2.1 Privacy Policy and Data Rights

- Link to privacy policy from every page — typically in footer (rule 15).
- Document the process for users to access and modify personal data (rule 16).

### 2.2 Account Management

- Allow account creation without requiring third-party identity provider as only option (rule 17).
- Require confirmation email for account creation (rule 18).
- Implement mechanism to prevent account theft — email verification, CAPTCHA, rate limiting (rule 19).
- Accounts opened online must be closeable online (rule 20).
- Users must be able to download their personal content (rule 21).
- Allow login with single credentials across all services (rule 22).
- Provide clear logout mechanism from all authenticated areas (rule 23).

### 2.3 Technical Privacy Requirements

- Accept email addresses containing `+` sign (rule 24).
- Send `Referrer-Policy` HTTP header on all responses (rule 25).

```http
Referrer-Policy: strict-origin-when-cross-origin
```

- Do not disclose whether a user account exists on login failure or password reset (rule 26).
- Transmit sensitive data over HTTPS (rule 27).
- Never include sensitive data in URL query strings — use POST (rule 28).
- Explain cookie purposes and consequences of refusing each category (rule 29).

---

## 3. E-Commerce (Rules 30-68)

### 3.1 Purchase Flow and Cart

- Allow purchasing without requiring account creation (rule 30).
- Provide link to item details from shopping cart (rule 31).
- Never add products to cart without explicit user action (rule 32).
- Do not pre-check subscription or opt-in checkboxes (rule 33).
- Display product availability before final confirmation (rule 34).
- Show estimated delivery time before final confirmation (rule 35).
- Show estimated delivery cost before final confirmation (rule 36).
- Describe digital product delivery method before order (rule 37).
- Allow quantity changes and item removal until final confirmation (rule 38).

### 3.2 Product Information and Pricing

- Describe nature and measurable characteristics of products (rule 39).
- Indicate validity period and conditions of special offers (rule 40).
- Display detailed sub-total inclusive of all charges before confirmation (rule 41).
- Provide financing conditions when offered (rule 42).
- Describe after-sales service conditions (rule 43).
- State debit and payment conditions before order (rule 44).
- Indicate warranty conditions (rule 45).
- Link to terms of sale from all pages (rule 46).
- State delivery zones or service areas (rule 47).
- List accepted payment methods and procedures (rule 48).
- Identify third parties involved in transactions (rule 49).
- Include dispute recourse information in terms (rule 50).
- State return address and conditions before finalising return (rule 51).
- Indicate return cost before finalising return (rule 52).
- Describe complaint process and timeline (rule 53).
- State reimbursement conditions (rule 54).
- List required hardware/software for digital services (rule 55).
- Display prices with applicable taxes and additional charges (rule 56).

### 3.3 Order Process

- Allow separate shipping and billing addresses (rule 57).
- Accept at least two payment methods (rule 58).
- Store payment details only after explicit consent (rule 59).
- Allow stored payment details to be modified or deleted (rule 60).
- Display transaction reference after order (rule 61).
- Make invoices available online (rule 62).
- Send confirmation email for each invoice (rule 63).
- Link to source when claiming affiliation with professional body (rule 64).
- Differentiate unavailable products from available ones (rule 65).
- Send order confirmation email with transaction reference (rule 66).
- Issue acknowledgement of receipt for each complaint (rule 67).
- Indicate origin of products where relevant (rule 68).

---

## 4. Forms (Rules 69-98)

### 4.1 Labels and Instructions

- Every form field must have a programmatically associated label — `<label for="...">` or `aria-labelledby` (rule 69).
- Supplementary instructions must be associated via `aria-describedby` (rule 70).
- Each label must indicate whether field is required (rule 71).
- Indicate expected input formats — e.g., "DD/MM/YYYY" (rule 72).
- Warn when a field is case-sensitive (rule 73).
- Include maximum character length in label or hint when limit applies (rule 74).

```html
<label for="postcode">
  Postcode <span aria-hidden="true">*</span>
  <span class="hint" id="postcode-hint">Required. Format: SW1A 1AA. Max 8 characters.</span>
</label>
<input id="postcode" name="postcode" type="text" maxlength="8"
  aria-required="true" aria-describedby="postcode-hint" />
```

### 4.2 Passwords

- Show password-strength indicator on creation (rule 75).
- Provide toggle to reveal/hide password (rule 76).

### 4.3 Visual Association

- Each label must be visually adjacent to its field (rule 77).
- Contextual information must appear close to the field (rule 78).

### 4.4 Error Handling

- Identify all fields with rejected data after failed submission — mark with `aria-invalid="true"` (rule 79).
- Explain why each piece of data was rejected (rule 80).
- Preserve all previously entered data on form rejection (rule 81).
- Write error messages in the form's language (rule 82).

```html
<input id="email" type="email" aria-invalid="true" aria-describedby="email-error" />
<p id="email-error" role="alert">Enter a valid email address, e.g. user@example.com.</p>
```

### 4.5 Multi-Step Processes

- Show summary of all data before final submission (rule 83).
- Allow return to normal navigation after submission without resubmitting (rule 84).
- Confirm whether submission succeeded or failed (rule 85).
- Warn of required documents at start of complex process (rule 86).
- Show numbered step list for complex processes (rule 87).
- Indicate current step clearly (rule 88).
- Allow going back to previous step (rule 89).
- Warn that browser back may cause data loss (rule 90).
- Preserve data when navigating between steps (rule 91).

### 4.6 Usability and Technical

- Permit paste into all form fields (rule 92).
- Group related options with `<optgroup>` (rule 93).
- Present list options in logical order (rule 94).
- Use appropriate HTML input types — `email`, `tel`, `url`, `date`, `search`, `password` (rule 95).

```html
<input type="email" name="email" autocomplete="email" />
<input type="tel" name="phone" autocomplete="tel" />
<input type="url" name="website" />
<input type="date" name="birthdate" autocomplete="bday" />
```

- Allow two-factor auth processes to be restarted on timeout (rule 96).
- Mark fields with appropriate `autocomplete` attribute (rule 97).
- Do not hide disabled buttons from screen readers — use `disabled` attribute not `aria-hidden` (rule 98).

---

## 5. Identification and Contact (Rules 99-115)

- Homepage must explain nature of content and services (rule 99).
- State audience restrictions on homepage if applicable (rule 100).
- Identify the responsible author, company, or organisation (rule 101).
- Each page title must identify the website — "Page name | Site name" (rule 102).
- Each page title must identify the page content (rule 103).
- Include `<link rel="icon">` in every page `<head>` (rule 104).

```html
<title>About Us | Acme Corporation</title>
<link rel="icon" type="image/svg+xml" href="/favicon.svg" />
```

- Provide complete address and phone number from all pages — typically footer (rule 105).
- Display company registration number where required by law (rule 106).
- Offer at least two contact methods (rule 107).
- Indicate response times for information requests (rule 108).
- State operating hours and service costs (rule 109).
- Send acknowledgement of receipt for every information request (rule 110).
- All transactional emails must include at least one contact method (rule 111).
- Provide at least one way to reach customer service (rule 112).
- Provide at least one way to reach the moderator of public spaces (rule 113).
- Identify person or team responsible for site content (rule 114).
- When claiming conformance with a standard, link to that standard (rule 115).

---

## 6. Images and Media (Rules 116-127)

### 6.1 Text Alternatives

- Decorative images: `alt=""` (rule 116).
- Image links: `alt` describes link destination, not the image (rule 117).
- Informative images: `alt` conveys the image's meaning (rule 118).

```html
<img src="divider.png" alt="" />
<a href="/home"><img src="logo.png" alt="Acme Corporation — Home" /></a>
<img src="sales-chart.png" alt="Bar chart showing monthly sales; December was highest at 1,200 units." />
```

- Thumbnails must not be full-size images resized in browser — serve appropriately sized images (rule 119).
- Embedded objects (`<object>`, `<embed>`) must have text alternatives (rule 120).

### 6.2 Audio and Video

- Provide text transcript for all audio and video (rule 121).
- Videos must have synchronised captions (rule 122).
- Indicate duration of audio and video content (rule 123).
- Videos must not autoplay (rule 124).
- Audio must not autoplay (rule 125).
- Video, animation, audio must be pausable (rule 126).
- Running video/animation must not block navigation (rule 127).

```html
<figure>
  <video controls width="800">
    <source src="demo.mp4" type="video/mp4" />
    <track kind="captions" src="demo.vtt" srclang="en" label="English captions" default />
  </video>
  <figcaption>Product demo (3:45). <a href="demo-transcript.html">Read the full transcript</a>.</figcaption>
</figure>
```

---

## 7. Internationalisation (Rules 128-135)

- Include international dialling codes for all phone numbers — e.g., +1 (212) 555-0100 (rule 128).
- Specify country in all mailing addresses (rule 129).
- Indicate language of link target when different from current page — use `hreflang` (rule 130).
- Declare main language in `<html lang="...">` (rule 131).
- Mark language changes within content using `lang` attribute (rule 132).

```html
<html lang="en">
<p>The phrase <span lang="fr">savoir-faire</span> means a knack or skill.</p>
```

- Links to translations must point to translated equivalent, not homepage (rule 133).
- Links to alternate language versions must be written in target language — "Français" not "French" (rule 134).
- Respect `Accept-Language` HTTP header for preferred language (rule 135).

---

## 8. Links (Rules 136-152)

### 8.1 Accessible Link Text

- Every link must have a `title` attribute or accessible name (rule 136).
- Link labels must describe function or target — avoid "click here", "read more" (rule 137).

```html
<!-- Wrong -->
<a href="/report.pdf">Click here</a>
<!-- Right -->
<a href="/report.pdf">2025 Annual Report (PDF, 1.2 MB)</a>
```

### 8.2 Visual Differentiation

- Links of same type must have consistent colour, shape, behaviour (rule 138).
- Underlines reserved for links only (rule 139).
- Links must be visually distinguishable from surrounding content (rule 140).
- Differentiate visited and unvisited links (rule 141).
- Differentiate internal from external links (rule 142).
- Differentiate links to restricted content (rule 143).

### 8.3 Link Behaviour and Context

- Links opening external software must have explicit label (rule 144).
- Telephone numbers must use `tel:` protocol (rule 145).

```html
<a href="tel:+12125550100">+1 (212) 555-0100</a>
```

- Warn when link opens in new window/tab — add "(opens in new tab)" (rule 146).
- Indicate file format for downloads (rule 147).
- Indicate file size for internal downloads (rule 148).
- Indicate language of downloadable file when different from page (rule 149).
- Name downloadable files so content and origin are identifiable from filename (rule 150).
- Do not restrict creation of inbound links (rule 151).
- All internal links must resolve to valid pages — run automated link checking (rule 152).

---

## 9. Navigation (Rules 153-172)

### 9.1 Access and Orientation

- Public content must be accessible without forced login (rule 153).
- Navigation must not cause pop-ups (rule 154).
- Every page must provide a way to return to homepage (rule 155).
- Show user location in site hierarchy — breadcrumbs or equivalent (rule 156).
- Active navigation items must be visually identifiable (rule 157).
- Navigation blocks must appear in same location on every page (rule 158).
- Navigation icons must have visible text label (rule 159).

### 9.2 Modal and Window Management

- Close buttons must be visually linked to content they close (rule 160).
- Close buttons must be immediately available without scrolling (rule 161).
- New windows and modals must have explicit close button (rule 162).
- Close mechanisms must be in same location on every page (rule 163).

```html
<dialog aria-labelledby="dialog-title">
  <h2 id="dialog-title">Confirm deletion</h2>
  <p>This action cannot be undone.</p>
  <button type="button" autofocus>Cancel</button>
  <button type="button" class="destructive">Delete</button>
  <button type="button" aria-label="Close dialog" class="close-btn">✕</button>
</dialog>
```

### 9.3 Keyboard Navigation

- Provide skip links — at minimum "Skip to main content" (rule 164).
- Keyboard focus must never be hidden — do not set `outline: none` without visible replacement (rule 165).
- All content and services must be operable by keyboard (rule 166).
- Keyboard navigation order must be logical and predictable (rule 167).

```css
:focus-visible {
  outline: 3px solid #005fcc;
  outline-offset: 2px;
}
```

### 9.4 Search

- Provide internal search engine for content-rich sites (rule 168).
- Search results pages must have stable, bookmarkable URLs (rule 169).
- Allow users to refine or relaunch search from results page (rule 170).

### 9.5 Site Map and Time Limits

- Provide sitemap accessible from every page (rule 171).
- Clearly indicate time limits on user actions or access (rule 172).
