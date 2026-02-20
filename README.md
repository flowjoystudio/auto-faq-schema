# Auto FAQ Schema

Automatically generates [FAQ schema markup](https://schema.org/FAQPage) (`application/ld+json`) from existing page content. Built for Webflow but works on any HTML page.

---

## Installation

Add the following `<script>` tag to your page's `<head>` or before `</body>`. Replace `@1.0.0` with the latest release version.

```html
<script src="https://cdn.jsdelivr.net/gh/flowjoystudio/auto-faq-schema@1.0.0/auto-faq-schema.js"></script>
```

---

## Usage

There are two ways to use this script depending on your page structure.

---

### Method 1 — Attribute-based (recommended)

Best used with **CMS collection lists** or any repeating structure where you can add custom attributes to individual elements.

Add `faq=parent` to the wrapper element, then `faq=question` and `faq=answer` to the respective child elements.

```html
<div faq="parent">
  <div> <!-- collection item, repeated -->
    <h3 faq="question">What is your return policy?</h3>
    <div faq="answer">We offer 30 days returns on all orders.</div>
  </div>
  <div>
    <h3 faq="question">How long does shipping take?</h3>
    <div faq="answer">Typically 3-5 business days.</div>
  </div>
</div>
```

> **Note:** The `faq=answer` element can be a rich text wrapper — the script reads `textContent` which strips HTML tags, producing clean plain text suitable for schema markup.

---

### Method 2 — Heading-based

Best used with **rich text blocks** where you cannot add attributes to individual elements. Add `faq=parent` to the rich text wrapper element.

The script looks for a heading with the exact text **"FAQ"** or **"Frequently Asked Questions"** to start collecting. After that, any heading ending in `?` is treated as a question, and the paragraph immediately following it is used as the answer.

```html
<div faq="parent">
  <!-- rich text content renders like this in the DOM -->
  <h2>FAQ</h2>

  <h3>What is your return policy?</h3>
  <p>We offer 30 days returns on all orders.</p>

  <h3>How long does shipping take?</h3>
  <p>Typically 3-5 business days.</p>
</div>
```

> **Note:** This method only reads plain `<p>` tags as answers. It won't capture rich text formatted answers (links, bold text etc.) — use Method 1 for those.

---

## How it works

Once the DOM is loaded the script:

1. Finds all elements with the `faq=parent` attribute
2. Collects question and answer pairs using whichever method applies
3. Builds a valid `FAQPage` schema object
4. Injects it into the `<head>` as a `<script type="application/ld+json">` tag

Both methods can be used on the same page across different parent elements.

---

## Webflow setup

**Attribute-based method**
- Add `faq` / `parent` as a custom attribute on the collection list wrapper
- Add `faq` / `question` on the question element
- Add `faq` / `answer` on the answer element (can be a rich text block)

**Heading-based method**
- Add `faq` / `parent` directly on the rich text element
- Structure your rich text with an "FAQ" heading followed by question headings (ending in `?`) and paragraph answers

---

## Known limitations

- The heading-based method requires the exact trigger text "FAQ" or "Frequently Asked Questions" to be present as a heading
- If both methods are used on the same `faq=parent` element, questions may be duplicated in the schema output — use one method per parent
- Answer content is always output as plain text (HTML tags are stripped)
