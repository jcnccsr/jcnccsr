---
layout: page
title: CSS Flexbox 
description: >
  This chapter covers the CSS box view, block and inline displays.
hide_description: true
sitemap: false
---

0. this unordered seed list will be replaced by toc as unordered list
{:toc}

## The Box Model, Block & Inline

### 1. The CSS Box Model

Every HTML element is represented as a rectangular box. Its layout and dimensions are affected by four layers:

- **Content:** The text, image, or other content inside the element.
- **Padding:** Space between the content and the border.
- **Border:** Surrounds the padding and content.
- **Margin:** Space outside the border, separating neighboring elements.

```css
.box {
  width: 200px;
  padding: 20px;
  border: 5px solid black;
  margin: 10px;
}
```

#### `content-box` vs. `border-box`

**Default: `content-box`**

The declared `width` and `height` apply only to the content. Padding and borders increase the final dimensions.

Total width = content + padding + border

For the example above:

`200 + 40 + 10 = 250px` total width, excluding margins.

**`border-box`**

The declared `width` and `height` include the content, padding, and border.

```css
*,
*::before,
*::after {
  box-sizing: border-box;
}
```

With `width: 200px`, the box remains 200px wide, excluding margins. The content area shrinks to accommodate padding and borders.

`border-box` is commonly used because declared dimensions are easier to reason about.

### 2. Margin and Padding

```css
.box {
  margin: 20px;
  padding: 10px 20px;
}
```

- `margin`: Controls space outside the border.
- `padding`: Controls space inside the border.

Shorthand values follow this order:

| Values | Meaning |
|---|---|
| `10px` | All four sides |
| `10px 20px` | Top/bottom, left/right |
| `10px 20px 30px` | Top, left/right, bottom |
| `10px 20px 30px 40px` | Top, right, bottom, left |

Individual sides can also be set using `margin-top`, `padding-left`, and similar properties.

#### Important behavior

- Vertical margins between block elements can collapse into a single margin rather than add together.
- Horizontal margins generally do not collapse.
- `margin: 0 auto` can horizontally center a block with a constrained width when sufficient space is available.
- Padding cannot be negative; margins can.
- Margins can be used to control spacing between elements, while padding creates internal breathing room.

### 3. Normal Flow

**Normal flow** is the browser's default layout behavior before elements are repositioned using CSS layout techniques.

By default, block elements stack vertically, while inline elements flow alongside text and other inline content.

Understanding normal flow is essential before learning Flexbox, Grid, and positioning.

### 4. Block vs. Inline Elements

#### Block elements

Typically begin on a new line and occupy the available width of their containing block.

Examples: `<div>`, `<p>`, `<h1>`, `<section>`.

- Stack vertically in normal flow.
- `width` and `height` generally apply.
- Padding, borders, and margins affect their boxes.

#### Inline elements

Flow within a line of text and occupy only the space their content requires.

Examples: `<span>`, `<a>`, `<strong>`, `<em>`.

- Do not normally start a new line.
- `width` and `height` do not apply in the usual way.
- Horizontal padding, borders, and margins work.
- Vertical padding and borders can extend beyond the line box; vertical margins do not change line spacing in the usual way.

#### Inline-block elements

Combine inline flow with block-like sizing behavior.

```css
.item {
  display: inline-block;
  width: 150px;
  height: 100px;
}
```

- Sit alongside other inline-level content.
- Allow explicit `width` and `height`.
- Support padding, borders, and margins like block boxes.

For more complex layouts, Flexbox or Grid is often a better choice.

### 5. `<div>` vs. `<span>`

Both are generic containers without inherent semantic meaning.

- `<div>`: A block-level container, useful for grouping sections or components.
- `<span>`: An inline container, useful for targeting part of a sentence.

```html
<div class="card">
  <h2>Learning CSS</h2>
  <p>Understand the <span class="highlight">box model</span>.</p>
</div>
```

Use semantic HTML elements when they accurately describe the content. Use `div` and `span` when a generic container is appropriate.

**Core principle:** Understand how an element's dimensions are calculated and how it participates in normal flow before attempting more advanced layouts.
