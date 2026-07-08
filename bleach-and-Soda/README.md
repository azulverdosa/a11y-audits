# Bleach & Soda Hair Studio — Accessibility Audit

**Site:** [bleachandsodahairstudio.com](https://www.bleachandsodahairstudio.com)  
**Type:** Small business — hair salon  
**Scope:** Homepage and embedded booking flow  
**Standard:** WCAG 2.1 Level AA  
**Tested:** April 2026  
**Tester:** Ava  
**Tools:** HeadingsMap · axe DevTools · WAVE · WebAIM Contrast Checker · VoiceOver (macOS)

## Executive Summary

Bleach & Soda Hair Studio has a solid foundation from an accessibility perspective. The site includes several features that are often missing on small business websites, including a skip link, properly labelled social media icons, good colour contrast throughout most of the homepage, and the correct page language.

The biggest accessibility concerns appear in the embedded Mangomint booking widget. While users can browse the main website without too much difficulty, someone relying on only a keyboard or a screen reader would encounter significant barriers when trying to book an appointment. Because booking is the primary purpose of the website, these issues have the biggest impact on real users.

The good news is that many of the issues on the website itself are relatively straightforward to fix. Improvements such as adding a proper heading structure, increasing colour contrast in one section, replacing generic "Learn More" links with descriptive text, and reviewing image alt text would noticeably improve the experience for assistive technology users.

Most of the remaining issues are contained within the third-party booking widget and will need to be raised with Mangomint.

**Findings by severity:**

| Severity | Count |
|---|---|
| Critical | 4 |
| Serious | 8 |
| Moderate | 3 |

**Priority remediation:**
1. Contact Mangomint — report keyboard and form accessibility failures, request VPAT or evaluate alternative booking tools
2. Add `h1`, correct heading hierarchy — site owner, low effort, high impact
3. Fix lavender section contrast — site owner, CSS change only
4. Rewrite AI-generated alt text — site owner, content change
5. Rewrite "Learn More" links with descriptive text — site owner

## What's Working Well

- Skip link is available, making it easier for keyboard users to jump straight to the main content.
- Page language is correctly set to English (Canada).
- The page has a descriptive title.
- Social media icons include accessible labels for screen reader users.
- Most text colour combinations meet WCAG contrast requirements.
- The logo includes appropriate alternative text.
- Navigation and footer links are clear and descriptive.
- Contact information is provided as readable text rather than embedded in an image.
- Keyboard navigation follows a logical order throughout the homepage.
- Calls to action such as "Book Now" clearly communicate their purpose.

## 1. Keyboard Navigation

### Overview
The main website navigation works reasonably well; however, there are several issues affecting the booking experience. The most significant barriers are within the third-party Mangomint booking widget, where some key actions cannot be completed using only a keyboard.

### K1 — Focus trap behind menu pop-out
**Severity:** Serious  
**WCAG:** 2.1.1 Keyboard (Level A), 2.4.3 Focus Order (Level A)  
**Responsibility:** Site owner / booking widget integration

**Issue:** 
When the booking panel opens, keyboard focus moves behind the menu instead of into the newly opened booking panel. This causes focus to remain in an area that is no longer visible or relevant to the user.

**Why it matters:** 
Keyboard users rely on focus indicators to understand where they are on a page. When focus moves somewhere unexpected, users may become confused or unable to continue navigating. For someone using only a keyboard, this can make the booking process feel broken because they cannot easily reach or interact with the newly opened booking experience.

**Suggested fix:** When the booking panel opens:

- Move keyboard focus into the first interactive element within the booking panel.
- Keep focus within the panel while it is open.
- Return focus to the original trigger element when the panel closes.

### K2 — Booking panel service options are not keyboard accessible
**Severity:** Critical  
**WCAG:** 2.1.1 Keyboard (Level A)  
**Responsibility:** Third-party (Mangomint)  

**Issue:** 
The service category options within the Mangomint booking widget are not available through keyboard navigation. The options are built using non-interactive elements rather than native controls such as buttons or links. As a result, users cannot reach or activate them using the Tab key, Enter, or Space.

**Why it matters:**
The booking flow is the primary purpose of the website. If someone cannot select a service using a keyboard, they cannot independently complete the booking process. This creates a complete barrier for keyboard-only users.

**Suggested fix:**
Interactive controls should use native HTML elements wherever possible.
Recommended approach:
-Replace interactive `<div>` elements with `<button>` elements.
- Ensure all controls have visible focus states.
- Ensure Enter and Space activate controls correctly.

If a custom component is required, it should include:
- Appropriate ARIA roles
- Keyboard event handling
- Proper focus management 

### K3 — Dropdown menu stays open when Shift+Tabbing backwards
**Severity:** Moderate  
**WCAG:** 2.1.1 Keyboard (Level A), 2.4.3 Focus Order (Level A)  
**Responsibility:** Site owner

**Issue:**
When the navigation dropdown is open, moving backwards through the page using Shift+Tab does not close the menu. Focus can move away from the dropdown while the menu remains visible. The Escape key also stops working in this state, requiring the user to return focus to the dropdown before it can be dismissed.

**Why it matters:**
Keyboard users expect menus to behave predictably. When a menu remains open after focus has moved away, it creates confusion and can make navigation feel inconsistent. Users may not know whether the menu is still active or how to close it.

**Suggested fix:** 
Update the dropdown behaviour so that:
- The menu closes when focus leaves the dropdown.
- Escape consistently closes the menu regardless of focus position.
- Focus behaviour matches common accessible menu patterns.

### Summary
| Check | Result |
|---|---|
| Can reach all interactive elements? | **PARTIAL** — links/buttons yes, page content no |
| Focus ring visible? | **PARTIAL** — visible where it exists, missing on booking widget toggle |
| Focus traps? | **FAIL** — see K1 |
| Tab order logical? | **PASS** |

## 2. Headings

### Overview
The homepage currently has some heading structure in place, but it is not organized correctly. The biggest issue is the absence of a main page heading (`<h1>`), which affects both accessibility and general page structure. 

### H1 — Missing main page `h1`
**Severity:** Serious  
**WCAG:** 2.4.6 Headings and Labels (Level AA)  
**Responsibility:** Site owner

**Issue:** 
The homepage does not contain an `<h1>` heading. The page begins with lower-level headings (`<h3>`) without first establishing a main heading that identifies the purpose of the page.

**Why it matters:**
The `<h1>` acts as the primary heading for a webpage. Screen reader users often use headings to quickly understand where they are and navigate through a page's content. Without a clear main heading, users may have difficulty understanding the purpose of the page when they first arrive. A proper heading structure also improves content organization for all users, not just people using assistive technology.

**Suggested fix:**
Add a single `<h1>` heading that describes the primary purpose of the page.

Possible options:

`<h1>Bleach & Soda Hair Studio</h1>`

or

`<h1>Welcome to Bleach & Soda Hair Studio</h1>`

The exact wording can be chosen based on the site's preferred messaging, but there should only be one primary page heading.

### H2 — Skipped heading levels
**Severity:** Serious  
**WCAG:** 1.3.1 Info and Relationships (Level A)  
**Responsibility:** Site owner

**Issue:** The page skips heading levels by moving directly to `<h3>` headings without an `<h1>` or `<h2>` structure above them.

**Why it matters:**
Headings create a hierarchy that helps users understand how content is organized.Screen reader users frequently navigate by jumping between headings. When heading levels are skipped, the page outline becomes harder to understand and users may miss the relationship between sections.

**Suggested fix:** Create a logical heading hierarchy - promote the salon name to `h1`. Promote section headings ("Who We Are", "Time Based", "Beauty Waste") to `h2`.

### H3 — Unclear "Time Based" heading text does not clearly describe the section 
**Severity:** Moderate  
**WCAG:** 2.4.6 Headings and Labels (Level AA)  
**Responsibility:** Site owner

**Issue:**
The heading "Time Based" does not clearly communicate what information is contained within the section. The section explains the salon's time-based pricing model, but the heading alone does not provide enough context.  

**Why it matters:** 
Users often scan pages by headings, especially screen reader users navigating through a heading list. A heading should provide enough information that someone can understand the purpose of the section without needing to read the surrounding content. 

**Suggested fix:** 
Update the heading to something more descriptive. "Time-based pricing" or "We price by time, not gender" would communicate the content more clearly. The final wording should match the site's brand voice while clearly describing the section purpose.

### Summary
| Check | Result |
|---|---|
| `h1` present? | **NO** |
| Logical nesting? | **NO** — page jumps directly to `h3` |
| Headings found | `h3`: "Who We Are" / "Time Based" / "Beauty Waste" |

## 3. Images

## Overview
Alternative text (alt text) helps ensure that people using screen readers receive the same meaningful information as sighted users.

The site has some good examples of appropriate alt text, including the logo and decorative imagery. However, two content images contain alt text that describes visual details rather than communicating the purpose or meaning of the image.

A pattern was also identified where image descriptions appear to have been generated from visual analysis rather than written based on the image's role on the page. Alt text should describe the purpose of an image, not simply what it looks like.

> **Site-wide pattern:** Images I1 and I2 show a consistent pattern of alt text that describes visual appearance without communicating purpose or context. This is characteristic of AI-generated alt text. All content images on the site should be reviewed and rewritten by a human who understands each image's intended meaning.

### I1 — Inaccurate alt text (likely AI-generated)
**Severity:** Serious  
**WCAG:** 1.1.1 Non-text Content (Level A)  
**Responsibility:** Site owner
**Issue:** 
The alt text for the "Who We Are" image appears to incorrectly identify the person in the image and includes uncertain descriptions of the surrounding environment.

Current alt text:
"Hairdresser with curly blonde hair in a black salon chair, getting her long, wavy hair styled or examined, in a salon with white walls and shelves."

`"Hairdresser"` misidentifies the subject — this appears to be a client, not a stylist. The phrase `"white walls or shelves"` (use of "or") suggests the text was generated without certainty rather than written by someone viewing the image. A screen reader user receives actively misleading information.  

Screenshot



**Suggested fix:** `alt="Client with curly blonde hair seated in a salon chair having their hair styled"`

### I2 — Alt text describes appearance, not purpose
**Severity:** Serious  
**WCAG:** 1.1.1 Non-text Content (Level A)  
**Issue:** The alt text for the "Who We Are" image appears to incorrectly identify the person in the image and includes uncertain descriptions of the surrounding environment.
Current alt text:

Hairdresser with curly blonde hair in a black salon chair...

The person appears to be a client rather than a hairdresser.

The wording also includes uncertainty ("white walls or shelves"), suggesting the description was generated from visual details rather than written with the image's purpose in mind.
**Suggested fix:** `alt="Hourglass icon representing time-based services"`

### Summary
| Image | Alt text | Result |
|---|---|---|
| Salon logo | `"Bleach & Soda Hair Studio"` | **PASS** |
| Decorative background | Empty `alt` attribute | **PASS** |
| "Who We Are" content image | `"Hairdresser with curly blonde hair..."` | **FAIL** — see I1 |
| "Time Based" content image | `"Two stylized cartoon eyes..."` | **FAIL** — see I2 |

## 4. Contrast

### Overview

### C1 — White text on lavender background fails contrast minimum
**Severity:** Serious  
**WCAG:** 1.4.3 Contrast Minimum (Level AA)  
**Issue:** White text on the lavender background section returns a contrast ratio of 2.9:1 — below the 3:1 minimum for large text and well below the 4.5:1 minimum for body text. All text in this section is affected.  
**Suggested fix:** Darken the lavender background significantly, or switch to dark text. Use [WebAIM Contrast Checker](https://webaim.org/resources/contrastchecker/) to find a passing combination before committing to a colour.

### Summary
#### Green background sections

| Element | Colours | Ratio | Result |
|---|---|---|---|
| `h3` (27px bold) | Dark grey/green on lime green | 8.75:1 | PASS |
| Body text (17px) | Dark grey/green on lime green | 8.75:1 | PASS |
| `h3` (27px bold) | Dark grey/red on lime green | 5.45:1 | PASS |

#### Lavender background section

| Element | Colours | Ratio | Required | Result |
|---|---|---|---|---|
| `h3` (27px bold) | White on lavender | 2.9:1 | 3:1 | FAIL |
| Body text (14.8px) | White on lavender | 2.9:1 | 4.5:1 | FAIL |

## 5. Links

| Group | Result |
|---|---|
| Nav links (Home, About, Services) | ✅ PASS |
| Logo link to homepage | ✅ PASS — established convention |
| "Book" / "Book Now" CTAs | ✅ PASS |
| Social media icons (header) | ✅ PASS — `aria-label` present |
| Footer nav links | ✅ PASS |
| Footer contact links | ✅ PASS |
| Footer social links (text) | ✅ PASS |

> **Worth noting (not a hard fail):** The "More" nav item opens a dropdown — the label is slightly vague and the dropdown keyboard behaviour (K3) compounds it.

### L1 — Two ambiguous "Learn More" links
**Severity:** Serious  
**WCAG:** 2.4.4 Link Purpose — In Context (Level A)  
**Issue:** Two identical "Learn More" links appear on the homepage. When read out of context via a screen reader links list, they are indistinguishable from each other.  
**Suggested fix:** Rewrite as descriptive text — e.g. "Learn more about our time-based pricing" and "Learn more about our team."

### L2 — Unlabelled link in Mangomint booking widget
**Severity:** Serious  
**WCAG:** 2.4.4 Link Purpose (Level A), 4.1.2 Name, Role, Value (Level A)  
**Responsibility:** Third-party (Mangomint)  
**Issue:** The widget contains a link to `mangomint.com` with no visible text, no `aria-label`, and no `aria-labelledby`. Screen readers announce it only as "link." Confirmed by axe DevTools.  
**Suggested fix:** Add `aria-label="Powered by Mangomint"` to the link element.

## 6. Forms

> **Note:** The booking form is delivered via an embedded third-party widget (Mangomint). Full form testing was partially blocked by the keyboard inaccessibility documented in K2. The following findings apply to form fields that were reachable. All issues in this section should be reported to Mangomint with a request for remediation or a VPAT.

> **Site-wide pattern:** Visible label text across the booking form is consistently placed in non-semantic elements (`<div>`, `<span>`) with no programmatic association to their inputs. This appears to be a framework-level pattern (React-based components) where visual design was prioritized over semantic HTML. All interactive form elements are affected.

---

### F1 — Form inputs have no programmatically associated labels
**Severity:** Critical  
**WCAG:** 1.3.1 Info and Relationships (Level A), 4.1.2 Name, Role, Value (Level A)  
**Responsibility:** Third-party (Mangomint)  
**Issue:** Input fields use a plain `<span>` with no attributes as a visual label. There is no `<label for="">`, no `aria-label`, and no `aria-labelledby`. A screen reader user landing on any input hears only the input type with no context about what is being asked. Clicking the visible label text does not focus the input.  
**Suggested fix:** Replace each `<span>` with `<label for="inputId">` matching the input's `id`. The `<label>` element is strongly preferred as it also enables click-to-focus behaviour.

---

### F2 — Radio button group has no `<fieldset>` and `<legend>`
**Severity:** Serious  
**WCAG:** 1.3.1 Info and Relationships (Level A)  
**Responsibility:** Third-party (Mangomint)  
**Issue:** The question "Would you like to receive appointment reminders?" is in a `<div>`, not a `<legend>`. The radio inputs have no `<fieldset>`. A screen reader user navigating to either radio button hears only the option label with no context about what question is being answered. Additionally, Shift+Tab into the group lands on the last option rather than the first, suggesting an unexpected focus order.  
**Suggested fix:**
```html
<fieldset>
  <legend>Would you like to receive appointment reminders?</legend>
  <label><input type="radio" name="sendReminderText" value="true"> Yes, send appointment reminders</label>
  <label><input type="radio" name="sendReminderText" value="false"> No</label>
</fieldset>
```

---

### F3 — Toggle has no visible focus indicator
**Severity:** Serious  
**WCAG:** 2.4.7 Focus Visible (Level AA)  
**Responsibility:** Third-party (Mangomint)  
**Issue:** The toggle uses a visually hidden native checkbox (`.react-toggle-screenreader-only`). When focused, the browser's native focus indicator renders at the off-screen position of the hidden input — appearing as a small outline in the bottom-left corner of the viewport, not on the visual toggle. Non-functional for sighted keyboard users.  
**Suggested fix:** Add a `:focus-visible` rule on the visual wrapper:
```css
.react-toggle:focus-within {
  outline: 2px solid #000;
  outline-offset: 2px;
}
```
This is a CSS-only fix requiring no library changes.

---

### F4 — Toggle has no programmatically associated label
**Severity:** Critical  
**WCAG:** 1.3.1 Info and Relationships (Level A), 4.1.2 Name, Role, Value (Level A)  
**Responsibility:** Third-party (Mangomint)  
**Issue:** The hidden checkbox has no `id`. The visible label text ("I agree to the cancellation policy") is in a plain `<span>` with no attributes. No `for`/`id` link, `aria-label`, or `aria-labelledby` exists. Screen readers announce this as an unlabelled checkbox.  
**Suggested fix:** Add `id="agreeToPolicy"` to the input. Replace the `<span>` with `<label for="agreeToPolicy">I agree to the cancellation policy</label>`.

---

### F5 — Required fields are not indicated
**Severity:** Serious  
**WCAG:** 3.3.2 Labels or Instructions (Level A)  
**Responsibility:** Third-party (Mangomint)  
**Issue:** No fields indicate they are required before the user attempts to submit. There is no asterisk, no "required" text, and no `aria-required="true"` on mandatory inputs.  
**Suggested fix:** Add a visible indicator (e.g. asterisk with a note: "Fields marked \* are required") and `aria-required="true"` or the native `required` attribute on each mandatory input.

---

### F6 — Form errors not announced to screen readers
**Severity:** Critical  
**WCAG:** 4.1.3 Status Messages (Level AA)  
**Responsibility:** Third-party (Mangomint)  
**Issue:** On failed submission, error messages appear visually but VoiceOver makes no announcement. No `aria-live` region exists on the error container, and focus does not move. A screen reader user has no feedback that submission failed.  
**Suggested fix:** Add `aria-live="assertive"` to the error container, or move focus to an error summary on failed submission.

---

### F7 — Error messages not associated with their inputs
**Severity:** Critical  
**WCAG:** 3.3.1 Error Identification (Level A), 1.3.1 Info and Relationships (Level A)  
**Responsibility:** Third-party (Mangomint)  
**Issue:** Error messages are in `<div>` elements with no `id`, no `role`, no `aria-live`. Inputs have no `aria-describedby`. No `aria-invalid="true"` is set on errored inputs. A screen reader user navigating to the field after failure hears nothing indicating an error exists.  
**Suggested fix:** Add a unique `id` to each error `<div>`. Add `aria-describedby="errorId"` to the corresponding input. Add `aria-invalid="true"` when the input is in an error state.

---

### F8 — Focus does not move on failed form submission
**Severity:** Serious  
**WCAG:** 2.4.3 Focus Order (Level A)  
**Responsibility:** Third-party (Mangomint)  
**Issue:** Focus remains on the Submit button after a failed submission. The user must manually navigate backwards through the form to discover errors. Combined with F6, a blind or keyboard-only user has no independent path to identifying and correcting errors.  
**Suggested fix:** Move focus to an error summary ("2 fields require your attention") or the first invalid field on validation failure.

---

### F9 — Error messages are not descriptive
**Severity:** Moderate  
**WCAG:** 3.3.1 Error Identification (Level A)  
**Responsibility:** Third-party (Mangomint)  
**Issue:** All error messages display only "required" — no field name, no instruction. Users who have moved focus away from the field cannot determine which field failed or what is needed to correct it.  
**Suggested fix:** Error messages should identify the field and describe the fix — e.g. "Email address is required" or "Please select a reminder preference."

> **Untested:** Successful form submission state was not tested to avoid creating a live booking. It is recommended to verify that confirmation messaging is announced to screen readers via `aria-live` or focus movement (WCAG 4.1.3).

---

## 7. Automated Testing — axe DevTools

**Run date:** April 16, 2026  
**URL:** https://www.bleachandsodahairstudio.com  
**Total issues:** 14 (1 serious, 13 moderate/best practice)

| Key finding | Manual finding | Status |
|---|---|---|
| Missing `h1` in Mangomint iframe | H1 | Confirmed ✅ |
| Unlabelled link in Mangomint iframe | L2 | Confirmed ✅ |
| 11× content outside landmark regions | A1 | New finding |

---

### A1 — Page content not contained by landmark regions
**Severity:** Moderate  
**WCAG:** 1.3.1 Info and Relationships (Level A)  
**Responsibility:** Third-party (Mangomint)  
**Issue:** axe DevTools identified 11 instances of content outside ARIA landmark regions (`<main>`, `<nav>`, `<header>`, `<footer>`), all within the Mangomint booking widget iframe. Screen reader users who navigate by landmarks will miss this content entirely.  
**Suggested fix:** All meaningful content should sit within a landmark region. The salon's own page should also be verified to ensure `<header>`, `<main>`, and `<footer>` are correctly used. Report to Mangomint.

---

## Findings Summary

| ID | Severity | Description | Owner |
|---|---|---|---|
| K1 | Serious | Focus trap behind menu pop-out | Site |
| K2 | Critical | Booking panel not keyboard accessible | Mangomint |
| K3 | Moderate | Dropdown stays open on Shift+Tab | Site |
| H1 | Serious | Missing `h1` | Site |
| H2 | Serious | Skipped heading levels | Site |
| H3 | Moderate | Unclear heading "Time Based" | Site |
| I1 | Serious | Inaccurate AI-generated alt text | Site |
| I2 | Serious | Alt text describes appearance only | Site |
| C1 | Serious | White on lavender fails contrast | Site |
| L1 | Serious | Two ambiguous "Learn More" links | Site |
| L2 | Serious | Unlabelled link in widget | Mangomint |
| F1 | Critical | Inputs have no associated labels | Mangomint |
| F2 | Serious | Radio group missing fieldset/legend | Mangomint |
| F3 | Serious | Toggle has no focus indicator | Mangomint |
| F4 | Critical | Toggle has no label | Mangomint |
| F5 | Serious | Required fields not indicated | Mangomint |
| F6 | Critical | Errors not announced to screen reader | Mangomint |
| F7 | Critical | Errors not linked to inputs | Mangomint |
| F8 | Serious | Focus lost on failed submission | Mangomint |
| F9 | Moderate | Error messages not descriptive | Mangomint |
| A1 | Moderate | Content outside landmark regions | Mangomint |
