# Responsive Web Design: Documentation

**Project:** Greenleaf Studio demo page
**Task:** Week 2 - Implementation of Responsive Web Design
**Files:** `index.html`, `styles.css`, `images/`

## 1. Overview

This project is a single-page website that adapts its layout to desktops, tablets and smartphones. It uses semantic HTML5, a mobile-first CSS stylesheet, CSS Grid and Flexbox, fluid typography, fluid images and media queries. No JavaScript is used, including for the mobile menu.

## 2. Planning and Design

I planned the page as a stack of sections (header, hero, services, gallery, about, contact, footer) and sketched how each should rearrange at three widths:

| Section | Phone (< 600px) | Tablet (600-899px) | Desktop (900px+) |
|---|---|---|---|
| Navigation | Hamburger menu, drops down | Hamburger menu | Horizontal links |
| Hero | Text above image, 1 column | 1 column | 2 columns |
| Services | 1 column | 2 columns | 4 columns |
| Gallery | 1 column | 2 columns | 3 columns |
| About | Stacked | Stacked | 2 columns |
| Contact form | 1 column | 2 columns (message full width) | 2 columns |
| Footer | Centered, stacked | Two columns | Two columns |

## 3. Methods Used

### 3.1 Semantic HTML
`header`, `nav`, `main`, `section`, `article`, `figure`/`figcaption` and `footer` give the page a clear structure. Each section has an `id` for anchor navigation, form fields have linked `label`s, images have `alt` text, and a "skip to main content" link helps keyboard users.

### 3.2 Viewport meta tag
`<meta name="viewport" content="width=device-width, initial-scale=1">` makes phones render at their real width rather than a zoomed-out desktop width. Without it, media queries would not behave as expected on mobile.

### 3.3 Mobile-first CSS
Base styles target phones. Larger layouts are added with `min-width` media queries, so small devices download and apply only what they need:

- `@media (min-width: 600px)` - tablet portrait
- `@media (min-width: 900px)` - tablet landscape and desktop
- `@media (min-width: 1200px)` - large desktop

### 3.4 Flexible grids
Layouts use CSS Grid with `fr` units and `gap`, for example:

```css
.grid { display: grid; grid-template-columns: 1fr; gap: var(--gap); }
@media (min-width: 600px) { .grid { grid-template-columns: repeat(2, 1fr); } }
@media (min-width: 900px) { .grid { grid-template-columns: repeat(4, 1fr); } }
```

Flexbox is used for the header and the statistics row (`flex-wrap: wrap` with `flex: 1 1 120px`), so items wrap naturally.

### 3.5 Fluid images
```css
img { max-width: 100%; height: auto; display: block; }
.gallery img { width: 100%; aspect-ratio: 4 / 3; object-fit: cover; }
```
Images never overflow their container. `width` and `height` attributes in the HTML let the browser reserve space and avoid layout shift. Images are SVG, so they stay sharp at any size, and gallery images use `loading="lazy"`.

### 3.6 Fluid typography and spacing
`clamp(min, preferred, max)` scales headings and section padding smoothly with the viewport, for example `h1 { font-size: clamp(2rem, 6vw, 3.5rem); }`. This avoids needing a breakpoint for every font-size change.

### 3.7 CSS-only mobile menu
A hidden checkbox plus a `label` (hamburger icon) toggles the menu using the `:checked ~` sibling selector and a `max-height` transition. At 900px and above the label is hidden and the menu becomes a horizontal flex list.

### 3.8 Custom properties
Colours, radius, shadow and gap are stored in `:root` variables, which keeps the stylesheet consistent and easy to change.

### 3.9 Additional queries
- `@media (max-height: 480px) and (orientation: landscape)` reduces hero padding on landscape phones.
- `@media (prefers-reduced-motion: reduce)` disables smooth scrolling and transitions for users who request it.
- `@media print` hides navigation and the form.

## 4. How the Design Adjusts on Different Devices

**Smartphone (e.g. 375px wide):** A green header with a hamburger button. Tapping it opens a full-width menu. All content is a single column, buttons and links have comfortable touch targets (at least 44px), and the gallery images are full width.

**Tablet (e.g. 768px wide):** Services and gallery show two cards per row, the contact form places Name and Email side by side, and the footer splits into two columns. The hamburger menu is still used.

**Desktop (e.g. 1280px wide):** The menu is horizontal, the hero shows text and illustration side by side, services show four cards in a row, the gallery shows three images, and the content is centred in a container with a maximum width of 1200px so lines stay readable.

## 5. Testing

The page is tested with the browser developer tools device toolbar (Chrome or Firefox) at these widths: 320, 375, 414, 600, 768, 900, 1024, 1280 and 1920 pixels, in portrait and landscape. At each width, check that:

- there is no horizontal scrolling at any width;
- text remains readable without zooming;
- images scale inside their containers;
- the menu works with mouse, touch and keyboard (Tab and Space);
- the layout changes exactly at the breakpoints.

## 6. Challenges and Strategies

| Challenge | Strategy |
|---|---|
| Horizontal scrolling on very narrow screens (320px) | Added `box-sizing: border-box`, `max-width: 100%` on images and avoided fixed pixel widths. |
| Hamburger menu without JavaScript | Used the checkbox-and-label technique with `:checked` and a `max-height` transition. |
| Images distorting in a grid | Used `aspect-ratio` with `object-fit: cover`. |
| Too many breakpoints for text sizes | Replaced them with `clamp()` for fluid font sizes and padding. |
| Sticky header covering anchor targets | Added `scroll-padding-top` on `html`. |
| Nav layout differing between mobile and desktop | Mobile-first base styles, with the desktop navigation added in a `min-width: 900px` query instead of overriding many rules. |

## 7. Conclusion

The page shows the main responsive techniques: flexible grids, fluid images, fluid type and media queries, applied mobile-first. The structure is clean semantic HTML, and the CSS is organised by section with design tokens, so it is easy to extend with more breakpoints or components.
