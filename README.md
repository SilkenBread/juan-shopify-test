# Luxury Jewelry PDP — Shopify Developer Technical Test

A custom **Product Detail Page** for a luxury jewelry e-commerce store, built on Shopify's Online Store 2.0 architecture. The page showcases an Emerald Cut Solitaire Engagement Ring with fully dynamic, merchant-editable sections.

> **Note:** For better visualization and review purposes, the full PDP was assembled on the homepage (`index.json`) template. A `product.pdp-luxury.json` template is also included for standard product page routing.

---

## Table of Contents

- [Live Sections](#live-sections)
- [Architecture & Data Strategy](#architecture--data-strategy)
- [Setup & Installation](#setup--installation)
- [Technical Decisions & Assumptions](#technical-decisions--assumptions)
- [Section Breakdown](#section-breakdown)
- [Bonus Features](#bonus-features)

---

## Live Sections

The PDP is composed of the following sections, rendered in order:

| # | Section | File |
|---|---------|------|
| 1 | Product Gallery + Product Info | `sections/product.liquid` |
| 2 | "You May Also Like" Carousel | `sections/product-recommendations.liquid` |
| 3 | Brand Story / Craftsmanship | `sections/release-notes.liquid` |
| 4 | Trust / Exceptional Quality | `sections/trust-badges.liquid` |
| 5 | Customer Reviews | `sections/customer-reviews.liquid` |
| 6 | FAQ Accordion | `sections/faqs.liquid` |
| 7 | Trust Badges Bar | `sections/trust-badges.liquid` |
| 8 | Follow Us / Social | `sections/follow-us.liquid` |
| 9 | Footer | `sections/footer.liquid` |

Additional supporting sections: `sections/header.liquid`, `sections/anouncement-bar.liquid`, `sections/cart.liquid`.

---

## Architecture & Data Strategy

### Metafields & Metaobjects

To ensure this PDP is not just visually accurate but also highly scalable and maintainable for a real-world merchant, I went beyond basic section schemas. I implemented a robust backend data structure using Shopify's **Metafields** and **Metaobjects** to manage complex content requirements.

- **Metaobjects (Structured & Reusable Content):** Instead of hardcoding "Customer Reviews" and "Trust Badges" into the theme files or relying on repetitive block schemas, I created custom Metaobjects. This establishes a centralized repository within the Shopify Admin where the merchant can manage reviews and trust features globally. The sections dynamically pull this structured data, ensuring consistency across the store.

- **Metafields (Product-Specific Data):** I utilized Metafields to handle data that falls outside standard variant constraints. For example, the "Shape" selector navigates between distinct sibling products (bypassing Shopify's variant limits while maintaining separate inventory/pricing). Metafields also dynamically populate the customer reviews section with `custom.average_rating`, `custom.review_count`, and `custom.customer_reviews`.

- **Collections & Complex Variants:** The "You May Also Like" carousel is powered by a dedicated Collection, and the product cards use `product.options_with_values` to intelligently group and display unique metal swatches without duplicating UI elements.

### OS 2.0 Section Architecture

- All custom sections are self-contained `.liquid` files with their own `{% schema %}` definitions.
- Every piece of content (text, images, links, colors) is editable via the **Theme Customizer** — zero hardcoded content.
- Block-based editing for repeatable elements (FAQ items, trust badges, metal options, product details tabs, shape selectors).
- JSON templates drive the page composition (`templates/index.json`, `templates/product.pdp-luxury.json`).

### Technology Stack

| Layer | Choice |
|-------|--------|
| Templates | Shopify Liquid |
| Carousel | Swiper.js v12 (CDN) |
| CSS | Vanilla CSS with custom properties, mobile-first |
| JavaScript | Vanilla JS (inline per section) |
| Fonts | Orpheus Pro (display), Inter / DM Sans (body) |
| Icons | Inline SVGs + asset SVGs |

---

## Setup & Installation

### Prerequisites

- [Shopify CLI](https://shopify.dev/docs/api/shopify-cli) installed
- A Shopify development store with products configured

### Steps

1. **Clone the repository:**
   ```bash
   git clone https://github.com/rinaldomelo/juan-shopify-test.git
   cd juan-shopify-test
   ```

2. **Connect to your store and preview:**
   ```bash
   shopify theme dev --store your-store.myshopify.com
   ```

3. **Set up product data in Shopify Admin:**
   - Create a product (e.g., "Emerald Cut Solitaire Engagement Ring") with variants for Metal and Size.
   - Upload product images (5+ recommended).
   - Configure **Metafields** on the product:
     - `custom.average_rating` (Rating) — e.g., 4.8/5
     - `custom.review_count` (Integer) — e.g., 126
     - `custom.customer_reviews` (List of mixed references) — link to Review metaobjects
   - Create **Metaobjects** for customer reviews with fields: title, description, date.
   - Create a Collection for the "You May Also Like" recommendations and assign related products.

4. **Assign the template:**
   - The PDP is rendered on the homepage (`index.json`) for review purposes.
   - Alternatively, assign `product.pdp-luxury.json` to a product via **Admin > Products > Theme template**.

5. **Customize via Theme Editor:**
   - Navigate to **Online Store > Customize** to edit all section content, images, colors, and blocks.

---

## Technical Decisions & Assumptions

- **Index-based PDP:** I built the full page on the homepage template (`index.json`) for easier visualization and review. This allows reviewers to see the complete page without needing to navigate to a specific product.

- **Swiper.js for carousels:** Chosen for its lightweight footprint, touch support, and broad compatibility. Loaded via CDN to avoid bundling overhead.

- **Shape selector as product navigation:** Since a single product can't hold all shape/cut variations as variants (Shopify's 100-variant and 3-option limit), each shape links to a separate product. This maintains independent inventory, pricing, and images per shape.

- **Inline CSS/JS:** All styles and scripts are inlined within their respective sections to keep them fully self-contained, per OS 2.0 best practices. Critical shared styles live in `assets/critical.css`.

- **Mobile-first responsive design:**
  - **Desktop (1280px+):** Two-column layout for gallery + product info.
  - **Tablet (768px–1279px):** Adapted layout with adjusted columns.
  - **Mobile (<768px):** Fully stacked, single-column with touch-friendly controls and Swiper galleries.

- **No third-party CSS frameworks:** All styling is vanilla CSS with CSS custom properties for theming consistency.

- **Accessibility:** Sections include `aria` labels, `alt` text on images, keyboard-navigable accordions (using native `<details>`/`<summary>`), and semantic HTML5 elements.

- **Image optimization:** All images use Shopify's `image_url` filter with proper sizing parameters. Below-the-fold images use lazy loading.

---

## Section Breakdown

### 1. Product Gallery & Product Info (`sections/product.liquid`)
- Large featured image with media support (images, video, external video).
- Thumbnail strip with horizontal scroll on mobile (Swiper carousel).
- Desktop: CSS grid two-column layout. Mobile: full Swiper gallery.
- Breadcrumb navigation, product title, star rating (from metafields), social sharing icons.
- Price display with Affirm financing info.
- **Variant selectors:** Metal type with visual image swatches, ring size dropdown, carat weight slider (1–5ct).
- **Shape selector:** Block-based, navigates between sibling products.
- Quantity selector with increment/decrement buttons.
- Full-width **Add to Cart** button (form-based submission).
- Collapsible product details: Description, Resizing, Shipping & Returns (block-based accordion).
- Matching products carousel (e.g., wedding bands).

### 2. "You May Also Like" (`sections/product-recommendations.liquid`)
- Pulls products from a configurable Shopify Collection.
- Swiper carousel with navigation arrows.
- 4 products visible on desktop, scrollable on mobile.
- Each card shows: product image, title, price, and metal option swatches.

### 3. Brand Story / Craftsmanship (`sections/release-notes.liquid`)
- Split layout: large image on one side, text content on the other (reversible via settings).
- Customizable heading, subtitle, body text, list items, and CTA button.
- All fields editable in Theme Customizer.

### 4. Trust / Exceptional Quality (`sections/trust-badges.liquid`)
- Block-based icon grid (2–3 columns).
- Each badge: SVG icon (HTML input), heading, short description.
- Responsive CSS Grid layout.

### 5. Customer Reviews (`sections/customer-reviews.liquid`)
- Displays average rating and review count from product metafields.
- Shows first 4 reviews with "Show More" expand functionality.
- Review image carousel (up to 4 images).
- Each review: reviewer name, star rating, title, description, verified badge.
- Sort dropdown for future extensibility.

### 6. FAQ Accordion (`sections/faqs.liquid`)
- Native `<details>`/`<summary>` elements for accessible expand/collapse.
- Block-based: each FAQ item is a block with question and answer fields.
- Custom open/close icons.
- Content fully editable via Theme Editor blocks.

### 7. Trust Badges Bar
- Reused `trust-badges.liquid` section with different block content.
- Horizontal row of trust/feature icons with labels (e.g., Free Shipping, Lifetime Warranty).

### 8. Follow Us (`sections/follow-us.liquid`)
- Instagram-style image grid (up to 5 blocks).
- "Follow Us" CTA button with configurable URL.
- Responsive: 4 columns desktop, 2x2 grid mobile.

### 9. Footer (`sections/footer.liquid`)
- Multi-column layout: logo, address/hours, social links, navigation columns.
- Newsletter signup form (creates Shopify customer contact).
- Copyright information.
- Pulls navigation from `linklists.footer`.

### Additional: Cart (`sections/cart.liquid`)
- Simple cart page with item list and quantity controls.
- Order summary sidebar on desktop, stacked on mobile.
- Breadcrumb navigation, checkout and continue shopping buttons.

---

## Bonus Features

- **Metal type visual swatches** with product images for each option.
- **Carat weight interactive slider** (1–5ct range).
- **Matching products cross-sell** carousel (e.g., wedding bands paired with engagement rings).
- **Affirm financing** integration display.
- **Smooth Swiper transitions** on all carousels with touch/swipe support.
- **Accordion animations** on FAQ and product detail sections.
- **Cart page** for complete user flow demonstration.
- **Newsletter subscription** in footer.
- **Announcement bar** with customizable messaging.

---

## File Structure

```
.
├── assets/              # SVGs, critical.css
├── config/              # Theme settings schema & data
├── layout/              # theme.liquid, password.liquid
├── locales/             # Translation files
├── sections/            # All custom section .liquid files
├── snippets/            # Reusable components (product-card, divider, etc.)
├── templates/           # JSON templates (index, product, cart, etc.)
└── .shopify/            # Metafield definitions
```

---

## Custom Snippets

| Snippet | Purpose |
|---------|---------|
| `product-card.liquid` | Product card with variant swatch detection |
| `matching-product-card.liquid` | Simplified card for matching products carousel |
| `icon-info.liquid` | Info icon SVG component |
| `divider.liquid` | Decorative horizontal divider |
| `css-variables.liquid` | Global CSS custom properties |

---

*Built with Shopify OS 2.0 best practices, vanilla CSS/JS, and a focus on merchant editability.*
