# STYLE.md: Unified Design & Content Standards

---

## 1. Core Philosophy
We design for the user, not the institution. Our goal is to reduce cognitive load through consistency, clarity, and radical accessibility.

1. **User-First:** Start with user needs, not organizational structure.
2. **Plain Language:** If a 12-year-old can't understand it, it's a barrier.
3. **Inclusive by Default:** Refer to `ACCESSIBILITY.md` for all interaction and visual standards.
4. **Consistency is Trust:** AI and humans must use the same tokens, patterns, and vocabulary.

---

## 2. Content & Voice Standards
Derived from *UK GDS* and *Digital.gov* standards.

### 2.1 Voice and Tone
We use an **Authoritative Peer** tone: professional and knowledgeable, but accessible and supportive.

| Context | Tone | Strategy |
| :--- | :--- | :--- |
| **Onboarding** | Encouraging | Focus on the benefit to the user. |
| **Technical/Legal** | Precise | Be unambiguous; explain "why" if a rule is complex. |
| **Error States** | Calm/Helpful | Don't blame the user. Provide a clear path to resolution. |

### 2.2 Plain Language & Word Choice
Avoid "Government-ese" or "Corporate-speak." AI agents must prioritize these substitutions:

| Avoid (Bureaucratic) | Use (Plain Language) |
| :--- | :--- |
| Utilize / Leverage | Use |
| Facilitate / Implement | Help / Carry out |
| At this point in time | Now |
| In order to | To |
| Notwithstanding | Despite / Even though |
| Requirements | Rules / What you need |

### 2.3 Grammar & Mechanics
* **Active Voice:** "The department issued the permit" NOT "The permit was issued by the department."
* **Sentence Case:** Use sentence case for all headings and buttons (e.g., "Save and continue," not "Save and Continue").
* **Lists:** Use bullets for items. Use numbered lists only for sequential steps.

---

## 3. Design Foundations (Tokens & UI)
Inspired by the *California Design System* and *Home Office Digital* patterns.

### 3.1 Design Tokens (CSS/Variables)
Use these tokens to maintain a "Single Source of Truth."

| Category | Token Name | Value (Example) | Requirement |
| :--- | :--- | :--- | :--- |
| **Primary** | `--color-brand-primary` | `#003152` | 4.5:1 Contrast min |
| **Background**| `--color-bg-surface` | `#FFFFFF` | |
| **Success** | `--color-feedback-go` | `#00703c` | |
| **Error** | `--color-feedback-stop` | `#d4351c` | |
| **Spacing** | `--space-unit` | `8px` | Use multiples (2x, 4x) |

### 3.2 Typography & Readability
* **Font Scaling:** Use `rem` units to respect user browser settings.
* **Line Length:** Keep body text between 45–75 characters per line (approx. 600px max-width).
* **Line Height:** Minimum `1.5` for body text; `1.2` for headings.

---

## 4. Accessibility & Semantic Logic
This section implements the mandates in `ACCESSIBILITY.md` [[ACCESSIBILITY.md]],


* **Heading Hierarchy:** Must be logical. H1 → H2 → H3. Never skip levels for "style."
* **Alt-Text:** Must describe the *intent* of the image, not just the pixels. 
* **Interactive Elements:** Every button or link must have a focus state that is visually distinct (e.g., a high-contrast 3px outline).

---

## 5. Instructions for AI Agents
**Meta-Prompting Rules:** When generating content or code, the Agent must:

1. **Verify Tokens:** Only use the CSS variables defined in Section 3.1.
2. **Scan for Jargon:** Before finalizing text, run a "Plain Language" check against the table in Section 2.2.
3. **Reference Accessibility:** Before outputting a UI component, check `ACCESSIBILITY.md` for ARIA and keyboard navigation requirements.
4. **Markdown Formatting:** Always use semantic Markdown. Use GFM (GitHub Flavored Markdown) callouts (`> [!NOTE]`) for emphasis.

Also see: [[AGENTS.md]]

---

## 6. References & Inspiration
* [UK GDS Style Guide](https://www.gov.uk/guidance/style-guide/a-to-z)
* [California Design System](https://designsystem.webstandards.ca.gov/)
* [18F Content Guide](https://content-guide.18f.gov/)
* [Harvard Accessibility & Readability](https://accessibility.huit.harvard.edu/design-readability)
* [Content Design London](https://contentdesign.london/blog/)

---
*Created by [Your Name]. This document is a living file. Please submit a PR for updates.*
