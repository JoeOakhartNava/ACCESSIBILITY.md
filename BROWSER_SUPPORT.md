---
layout: page
title: Browser Support Policy
meta_title: Browser Version Support Guarantees | ACCESSIBILITY.md
description: Browser version support guarantees and testing requirements for accessible web applications
lede: Comprehensive browser and assistive technology support policy for projects adopting ACCESSIBILITY.md standards
source_url: https://github.com/mgifford/ACCESSIBILITY.md/blob/main/BROWSER_SUPPORT.md
---

# Browser Support Policy

## Overview

This document defines browser version support guarantees for projects implementing WCAG 2.2 AA accessibility standards. Following these guidelines ensures that accessibility features work consistently across modern browsers and assistive technologies.

## Browser Version Support Guarantees

Projects adopting this policy should support:

### Minimum Support Requirement

**Support the last 2 major releases** of all major browser engines:

| Browser Engine | Browsers | Current Support Policy |
| :--- | :--- | :--- |
| **Chromium/Blink** | Chrome, Edge, Opera, Brave | Last 2 major versions |
| **Gecko** | Firefox | Last 2 major versions |
| **WebKit** | Safari (macOS/iOS) | Last 2 major versions |

### Version Examples (as of February 2026)

| Browser | Minimum Supported Version | Latest Version |
| :--- | :--- | :--- |
| Chrome | 130 | 131 |
| Firefox | 132 | 133 |
| Safari | 17.4 | 18.0 |
| Edge | 130 | 131 |

**Note:** Update this table quarterly or when major versions are released.

### Extended Support Considerations

For **government or enterprise projects**, consider supporting:
- **Last 3 major versions** for all browsers
- **LTS (Long-Term Support) versions** if applicable (e.g., Firefox ESR)
- **Specific OS versions** (e.g., iOS 16+, Android 12+)

## Assistive Technology Support Matrix

### Primary Testing Combinations

Test with these **browser + assistive technology (AT)** combinations:

| Assistive Technology | Version | Browser | OS | Priority |
| :--- | :--- | :--- | :--- | :--- |
| **JAWS** | Latest, Latest-1 | Chrome, Firefox | Windows | High |
| **NVDA** | Latest, Latest-1 | Chrome, Firefox | Windows | High |
| **VoiceOver** | Latest (built-in) | Safari | macOS | High |
| **VoiceOver** | Latest (built-in) | Safari | iOS | High |
| **TalkBack** | Latest (built-in) | Chrome | Android | Medium |
| **Narrator** | Latest (built-in) | Edge | Windows | Medium |

### Recommended Testing Schedule

- **Weekly:** Automated tests on all supported browser versions
- **Per release:** Manual testing with top 3 AT combinations (JAWS/NVDA/VoiceOver)
- **Quarterly:** Full AT matrix testing including mobile devices
- **When browser updates:** Regression testing within 2 weeks of major browser release

## Browser Feature Requirements

### Essential Features for Accessibility

All supported browsers must support:

- **HTML5 Semantic Elements:** `<main>`, `<nav>`, `<article>`, `<section>`, `<header>`, `<footer>`
- **ARIA 1.2:** All ARIA roles, states, and properties
- **CSS3:** Media queries, flexbox, grid, custom properties
- **JavaScript ES6+:** Modern DOM APIs, Promises, async/await
- **Web Standards:**
  - Focus management APIs
  - Intersection Observer (lazy loading)
  - Custom Elements (if using web components)
  - CSS Grid and Flexbox (layout)

### Known Browser Limitations

Document known accessibility issues per browser:

#### Safari/WebKit
- VoiceOver announcement delays with live regions (known issue in Safari < 17)
- Focus indicator styling inconsistencies

#### Firefox/Gecko
- ARIA `aria-description` support added in Firefox 115

#### Chrome/Chromium
- Focus-visible pseudo-class behavior differences

**Action:** Maintain a public list of known issues with workarounds in your project documentation.

## Testing Requirements

### Automated Testing

Use these tools in your CI/CD pipeline:

1. **axe-core** (via axe DevTools, jest-axe, cypress-axe)
   - Run on last 2 major versions of Chrome, Firefox, Safari
2. **Lighthouse CI**
   - Minimum accessibility score: 90
3. **Pa11y** or **HTML_CodeSniffer**
   - WCAG 2.2 AA validation

### Manual Testing Checklist

For each supported browser:

- [ ] Keyboard navigation (Tab, Shift+Tab, Arrow keys, Enter, Space)
- [ ] Focus indicators visible and clear
- [ ] Screen reader announces all interactive elements
- [ ] Form validation messages are announced
- [ ] Dynamic content updates announced via live regions
- [ ] No keyboard traps
- [ ] Skip links work correctly
- [ ] Color contrast meets WCAG 2.2 AA (4.5:1 text, 3:1 UI components)

### Browser Testing Tools

#### Local Development
- **BrowserStack** (cross-browser testing)
- **Sauce Labs** (automated + manual testing)
- **LambdaTest** (real device testing)

#### CI/CD Integration
```yaml
# Example GitHub Actions matrix
strategy:
  matrix:
    browser: [chrome, firefox, safari]
    version: [latest, latest-1]
```

See [examples/A11Y_SHIFT_LEFT_WORKFLOW.yml](./examples/A11Y_SHIFT_LEFT_WORKFLOW.yml) for implementation guidance.

## Deprecation Policy

### When to Drop Browser Support

Drop support for a browser version when:

1. **Usage drops below 2%** in your analytics (or 0.5% globally)
2. **Vendor ends security updates** (e.g., IE11 ended June 2022)
3. **Critical accessibility bugs cannot be worked around**

### Deprecation Process

1. **Announce 90 days in advance** in release notes
2. **Document alternative solutions** for affected users
3. **Keep compatibility for one additional release cycle** (grace period)
4. **Remove code and update documentation** in subsequent release

## Implementation Checklist

For projects adopting this browser support policy:

- [ ] Add browser support table to project README
- [ ] Configure automated testing for all supported browsers
- [ ] Document browser-specific workarounds or polyfills
- [ ] Set up analytics to track browser usage
- [ ] Establish quarterly review schedule for support policy
- [ ] Train team on AT testing procedures
- [ ] Add browser/AT testing to PR requirements
- [ ] Create issue templates for browser-specific bugs

## Resources

### Browser Release Schedules
- **Chrome:** [Release Schedule](https://chromiumdash.appspot.com/schedule)
- **Firefox:** [Release Calendar](https://wiki.mozilla.org/Release_Management/Calendar)
- **Safari:** Typically 2 releases per year (with iOS major releases)

### Assistive Technology Resources
- **JAWS Versions:** [Freedom Scientific](https://www.freedomscientific.com/products/software/jaws/)
- **NVDA Releases:** [NV Access](https://www.nvaccess.org/download/)
- **Screen Reader Support:** [PowerMapper Compatibility Tables](https://www.powermapper.com/tests/screen-readers/)

### Testing Guides
- [WebAIM Screen Reader Testing](https://webaim.org/articles/screenreader_testing/)
- [Accessibility Testing with NVDA](https://webaim.org/articles/nvda/)
- [VoiceOver Testing Guide](https://webaim.org/articles/voiceover/)

## Template: Project Browser Support Matrix

Copy this template into your project's README or ACCESSIBILITY.md:

```markdown
## Browser Support

We support the **last 2 major versions** of all major browsers:

| Browser | Minimum Version | Testing Status |
| :--- | :--- | :--- |
| Chrome | 130+ | ✅ Automated + Manual |
| Firefox | 132+ | ✅ Automated + Manual |
| Safari | 17.4+ | ✅ Automated + Manual |
| Edge | 130+ | ✅ Automated |

### Assistive Technology Testing

| Screen Reader | Browser | Status |
| :--- | :--- | :--- |
| JAWS 2024 | Chrome | ✅ Tested |
| NVDA 2024.4 | Firefox | ✅ Tested |
| VoiceOver (macOS) | Safari | ✅ Tested |
| VoiceOver (iOS) | Safari | ✅ Tested |

Last updated: [Date]
```

## Questions or Feedback

- **Discussions:** [GitHub Discussions](https://github.com/mgifford/ACCESSIBILITY.md/discussions)
- **Issues:** [Report an issue](https://github.com/mgifford/ACCESSIBILITY.md/issues)
- **Contributing:** See [CONTRIBUTING.md](./CONTRIBUTING.md)

---

**Last Updated:** 2026-02-23
