# Bleach & Soda Hair Studio — Accessibility Audit

**Site:** [bleachandsodahairstudio.com](https://www.bleachandsodahairstudio.com)  
**Type:** Small business — hair salon  
**Scope:** Homepage and embedded booking flow  
**Standard:** WCAG 2.1 Level AA  
**Tested:** April 2026  
**Tester:** Ava  
**Tools:** HeadingsMap · axe DevTools · WAVE · WebAIM Contrast Checker · VoiceOver (macOS)

## Executive Summary

Bleach & Soda Hair Studio was audited for WCAG 2.1 Level AA conformance across the homepage and embedded booking flow. The site demonstrates some genuine accessibility strengths — a skip navigation link is present, social media icons include correct `aria-label` attributes, contrast passes on the primary green background, and the page language is correctly set to `lang="en-CA"`. However, significant barriers exist that would prevent a keyboard-only or screen reader user from independently completing a booking — the site's core conversion action.

The most critical issues are concentrated in the third-party Mangomint booking widget, which is entirely keyboard inaccessible and contains multiple unlabelled form elements, missing error announcements, and no focus management on validation failure. A keyboard-only user cannot select a service or complete a booking independently. A screen reader user receives no feedback when form submission fails.

Several issues on the salon's own pages are directly remediable by the site owner without vendor involvement: the page lacks a heading structure (no `h1`, headings start at `h3`), white text on the lavender background section fails contrast minimums, and two "Learn More" links are indistinguishable when read out of context by a screen reader.

A consistent pattern of likely AI-generated alt text was identified across content images — text that describes visual surface details without communicating the image's meaning or purpose. All content images should be reviewed and rewritten by a human.

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

| Item | Detail |
|---|---|
| ✅ Skip navigation link | Present — uncommon on small business sites |
| ✅ Language attribute | `lang="en-CA"` correctly set on `<html>` |
| ✅ Page title | `<title>Bleach & Soda Hair Studio</title>` — descriptive |
| ✅ Social media icons | TikTok and Instagram have `aria-label` attributes |
| ✅ Contrast — green sections | Ratio 8.75:1 on primary green background |
| ✅ Logo alt text | Accurate and descriptive |
| ✅ Nav and footer links | Clear and self-descriptive |
| ✅ Footer contact info | Phone, email, address written as readable text |
| ✅ Tab order | Logical on the main page |
| ✅ CTAs | "Book Now" and header CTA are self-descriptive |

## 1. Keyboard Navigation

| Check | Result |
|---|---|
| Can reach all interactive elements? | **PARTIAL** — links/buttons yes, page content no |
| Focus ring visible? | **PARTIAL** — visible where present, missing on booking widget toggle |
| Focus traps? | **FAIL** — see K1 |
| Tab order logical? | **PASS** |

### K1 — Focus trap behind menu pop-out
**Severity:** Serious  
**WCAG:** 2.1.1 Keyboard (Level A), 2.4.3 Focus Order (Level A)  
**Detail:** The link to the booking form sets focus behind the menu pop-out. Focus becomes trapped in an unusable state.  
**Suggested fix:** Ensure focus moves into the booking panel when it opens, not behind it.

### K2 — Booking panel service options are not keyboard accessible
**Severity:** Critical  
**WCAG:** 2.1.1 Keyboard (Level A)  
**Responsibility:** Third-party (Mangomint)  
**Detail:** Service category options are rendered as non-interactive `<div>` elements. They are not reachable by Tab and cannot be activated by keyboard. A keyboard-only user cannot select a service and therefore cannot complete a booking independently — the site's core function.  
**Suggested fix:** Interactive elements must use `<button>` or `<a>`, or include `role="button"` with `tabindex="0"` and `keydown` handlers for Enter/Space. Native HTML elements are strongly preferred.  
**Note:** Report to Mangomint. Consider requesting their VPAT or evaluating an alternative booking tool with documented WCAG 2.1 AA support.

### K3 — Dropdown menu stays open when Shift+Tabbing backwards
**Severity:** Moderate  
**WCAG:** 2.1.1 Keyboard (Level A), 2.4.3 Focus Order (Level A)  
**Detail:** When a dropdown is open and focus moves backwards via Shift+Tab, the menu remains open and ESC stops working. The user must re-enter the dropdown to dismiss it.  
**Suggested fix:** Ensure dropdown closes when focus leaves in any direction.

## 2. Headings

| Check | Result |
|---|---|
| `h1` present? | **NO** |
| Logical nesting? | **NO** — page jumps directly to `h3` |
| Headings found | `h3`: "Who We Are" / "Time Based" / "Beauty Waste" |

### H1 — Missing `h1`
**Severity:** Serious  
**WCAG:** 2.4.6 Headings and Labels (Level AA)  
**Detail:** No `h1` exists on the page. Every page requires exactly one `h1` describing its primary purpose. Screen reader users rely on the `h1` to orient themselves when landing on a page. Confirmed by axe DevTools.  
**Suggested fix:** The salon name or "Welcome to Bleach & Soda Hair Studio" should be the `h1`.

### H2 — Skipped heading levels
**Severity:** Serious  
**WCAG:** 1.3.1 Info and Relationships (Level A)  
**Detail:** The page jumps from no `h1` directly to `h3`. Screen reader users navigate pages by jumping between headings — a broken structure means they cannot orient themselves or scan content efficiently.  
**Suggested fix:** Promote the salon name to `h1`. Promote section headings ("Who We Are", "Time Based", "Beauty Waste") to `h2`.

### H3 — Unclear heading text
**Severity:** Moderate  
**WCAG:** 2.4.6 Headings and Labels (Level AA)  
**Detail:** The heading "Time Based" does not clearly describe the content below it, which explains time-based (not gender-based) pricing. The heading is ambiguous out of context.  
**Suggested fix:** "Time-based pricing" or "We price by time, not gender" would communicate the content more clearly.

## 3. Images

| Image | Alt text | Result |
|---|---|---|
| Salon logo | `"Bleach & Soda Hair Studio"` | ✅ PASS |
| Decorative background | Empty `alt` attribute | ✅ PASS |
| "Who We Are" content image | `"Hairdresser with curly blonde hair..."` | ❌ FAIL — see I1 |
| "Time Based" content image | `"Two stylized cartoon eyes..."` | ❌ FAIL — see I2 |

> **Site-wide pattern:** Images I1 and I2 show a consistent pattern of alt text that describes visual appearance without communicating purpose or context. This is characteristic of AI-generated alt text. All content images on the site should be reviewed and rewritten by a human who understands each image's intended meaning.

### I1 — Inaccurate alt text (likely AI-generated)
**Severity:** Serious  
**WCAG:** 1.1.1 Non-text Content (Level A)  
**Detail:** `"Hairdresser"` misidentifies the subject — this appears to be a client, not a stylist. The phrase `"white walls or shelves"` (use of "or") suggests the text was generated without certainty rather than written by someone viewing the image. A screen reader user receives actively misleading information.  
**Suggested fix:** `alt="Client with curly blonde hair seated in a salon chair having their hair styled"`

### I2 — Alt text describes appearance, not purpose
**Severity:** Serious  
**WCAG:** 1.1.1 Non-text Content (Level A)  
**Detail:** The image appears to be an hourglass representing time-based pricing. The alt text describes visual surface details (cartoon eyes, colours, patterns) with no connection to the image's meaning or the section it illustrates. A screen reader user gains no understanding of why the image is there.  
**Suggested fix:** `alt="Hourglass icon representing time-based services"`

## 4. Contrast

### Green background sections

| Element | Colours | Ratio | Result |
|---|---|---|---|
| `h3` (27px bold) | Dark grey/green on lime green | 8.75:1 | ✅ PASS |
| Body text (17px) | Dark grey/green on lime green | 8.75:1 | ✅ PASS |
| `h3` (27px bold) | Dark grey/red on lime green | 5.45:1 | ✅ PASS |

### Lavender background section

| Element | Colours | Ratio | Required | Result |
|---|---|---|---|---|
| `h3` (27px bold) | White on lavender | 2.9:1 | 3:1 | ❌ FAIL |
| Body text (14.8px) | White on lavender | 2.9:1 | 4.5:1 | ❌ FAIL |

### C1 — White text on lavender background fails contrast minimum
**Severity:** Serious  
**WCAG:** 1.4.3 Contrast Minimum (Level AA)  
**Detail:** White text on the lavender background section returns a contrast ratio of 2.9:1 — below the 3:1 minimum for large text and well below the 4.5:1 minimum for body text. All text in this section is affected.  
**Suggested fix:** Darken the lavender background significantly, or switch to dark text. Use [WebAIM Contrast Checker](https://webaim.org/resources/contrastchecker/) to find a passing combination before committing to a colour.

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
**Detail:** Two identical "Learn More" links appear on the homepage. When read out of context via a screen reader links list, they are indistinguishable from each other.  
**Suggested fix:** Rewrite as descriptive text — e.g. "Learn more about our time-based pricing" and "Learn more about our team."

### L2 — Unlabelled link in Mangomint booking widget
**Severity:** Serious  
**WCAG:** 2.4.4 Link Purpose (Level A), 4.1.2 Name, Role, Value (Level A)  
**Responsibility:** Third-party (Mangomint)  
**Detail:** The widget contains a link to `mangomint.com` with no visible text, no `aria-label`, and no `aria-labelledby`. Screen readers announce it only as "link." Confirmed by axe DevTools.  
**Suggested fix:** Add `aria-label="Powered by Mangomint"` to the link element.

## 6. Forms

> **Note:** The booking form is delivered via an embedded third-party widget (Mangomint). Full form testing was partially blocked by the keyboard inaccessibility documented in K2. The following findings apply to form fields that were reachable. All issues in this section should be reported to Mangomint with a request for remediation or a VPAT.

> **Site-wide pattern:** Visible label text across the booking form is consistently placed in non-semantic elements (`<div>`, `<span>`) with no programmatic association to their inputs. This appears to be a framework-level pattern (React-based components) where visual design was prioritized over semantic HTML. All interactive form elements are affected.

---

### F1 — Form inputs have no programmatically associated labels
**Severity:** Critical  
**WCAG:** 1.3.1 Info and Relationships (Level A), 4.1.2 Name, Role, Value (Level A)  
**Responsibility:** Third-party (Mangomint)  
**Detail:** Input fields use a plain `<span>` with no attributes as a visual label. There is no `<label for="">`, no `aria-label`, and no `aria-labelledby`. A screen reader user landing on any input hears only the input type with no context about what is being asked. Clicking the visible label text does not focus the input.  
**Suggested fix:** Replace each `<span>` with `<label for="inputId">` matching the input's `id`. The `<label>` element is strongly preferred as it also enables click-to-focus behaviour.

---

### F2 — Radio button group has no `<fieldset>` and `<legend>`
**Severity:** Serious  
**WCAG:** 1.3.1 Info and Relationships (Level A)  
**Responsibility:** Third-party (Mangomint)  
**Detail:** The question "Would you like to receive appointment reminders?" is in a `<div>`, not a `<legend>`. The radio inputs have no `<fieldset>`. A screen reader user navigating to either radio button hears only the option label with no context about what question is being answered. Additionally, Shift+Tab into the group lands on the last option rather than the first, suggesting an unexpected focus order.  
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
**Detail:** The toggle uses a visually hidden native checkbox (`.react-toggle-screenreader-only`). When focused, the browser's native focus indicator renders at the off-screen position of the hidden input — appearing as a small outline in the bottom-left corner of the viewport, not on the visual toggle. Non-functional for sighted keyboard users.  
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
**Detail:** The hidden checkbox has no `id`. The visible label text ("I agree to the cancellation policy") is in a plain `<span>` with no attributes. No `for`/`id` link, `aria-label`, or `aria-labelledby` exists. Screen readers announce this as an unlabelled checkbox.  
**Suggested fix:** Add `id="agreeToPolicy"` to the input. Replace the `<span>` with `<label for="agreeToPolicy">I agree to the cancellation policy</label>`.

---

### F5 — Required fields are not indicated
**Severity:** Serious  
**WCAG:** 3.3.2 Labels or Instructions (Level A)  
**Responsibility:** Third-party (Mangomint)  
**Detail:** No fields indicate they are required before the user attempts to submit. There is no asterisk, no "required" text, and no `aria-required="true"` on mandatory inputs.  
**Suggested fix:** Add a visible indicator (e.g. asterisk with a note: "Fields marked \* are required") and `aria-required="true"` or the native `required` attribute on each mandatory input.

---

### F6 — Form errors not announced to screen readers
**Severity:** Critical  
**WCAG:** 4.1.3 Status Messages (Level AA)  
**Responsibility:** Third-party (Mangomint)  
**Detail:** On failed submission, error messages appear visually but VoiceOver makes no announcement. No `aria-live` region exists on the error container, and focus does not move. A screen reader user has no feedback that submission failed.  
**Suggested fix:** Add `aria-live="assertive"` to the error container, or move focus to an error summary on failed submission.

---

### F7 — Error messages not associated with their inputs
**Severity:** Critical  
**WCAG:** 3.3.1 Error Identification (Level A), 1.3.1 Info and Relationships (Level A)  
**Responsibility:** Third-party (Mangomint)  
**Detail:** Error messages are in `<div>` elements with no `id`, no `role`, no `aria-live`. Inputs have no `aria-describedby`. No `aria-invalid="true"` is set on errored inputs. A screen reader user navigating to the field after failure hears nothing indicating an error exists.  
**Suggested fix:** Add a unique `id` to each error `<div>`. Add `aria-describedby="errorId"` to the corresponding input. Add `aria-invalid="true"` when the input is in an error state.

---

### F8 — Focus does not move on failed form submission
**Severity:** Serious  
**WCAG:** 2.4.3 Focus Order (Level A)  
**Responsibility:** Third-party (Mangomint)  
**Detail:** Focus remains on the Submit button after a failed submission. The user must manually navigate backwards through the form to discover errors. Combined with F6, a blind or keyboard-only user has no independent path to identifying and correcting errors.  
**Suggested fix:** Move focus to an error summary ("2 fields require your attention") or the first invalid field on validation failure.

---

### F9 — Error messages are not descriptive
**Severity:** Moderate  
**WCAG:** 3.3.1 Error Identification (Level A)  
**Responsibility:** Third-party (Mangomint)  
**Detail:** All error messages display only "required" — no field name, no instruction. Users who have moved focus away from the field cannot determine which field failed or what is needed to correct it.  
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
**Detail:** axe DevTools identified 11 instances of content outside ARIA landmark regions (`<main>`, `<nav>`, `<header>`, `<footer>`), all within the Mangomint booking widget iframe. Screen reader users who navigate by landmarks will miss this content entirely.  
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
