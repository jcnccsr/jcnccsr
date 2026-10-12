---
layout: page
title: CSS Box Model, Block, & Inline
description: >
  Notes I keep about the CSS box model and block and inline displays.
hide_description: true
sitemap: false
---

0. this unordered seed list will be replaced by toc as unordered list
{:toc}

## The Box Model, Block, and Inline

### 1. The CSS Box Model

I picture every HTML element as a rectangular box. Four layers affect its layout and dimensions:

- **Content:** The text, image, or other content inside the element.
- **Padding:** Space I add between the content and the border.
- **Border:** The edge around the padding and content.
- **Margin:** Space outside the border that separates neighboring elements.

```css
.box {
  width: 200px;
  padding: 20px;
  border: 5px solid black;
  margin: 10px;
}
```

#### `content-box` vs. `border-box`

**The default is `content-box`**

`width` and `height` apply only to the content. Padding and borders make the final dimensions larger.

Total width = content + padding + border

For the example above, I get:

`200 + 40 + 10 = 250px` total width, excluding margins.

**With `border-box`**

The declared `width` and `height` include the content, padding, and border.

```css
*,
*::before,
*::after {
  box-sizing: border-box;
}
```

With `width: 200px`, the box stays 200px wide, not including margins. The content area shrinks to make room for padding and borders.

I find `border-box` easier to reason about because the declared dimensions match the overall box size.

### 2. Margin and Padding

```css
.box {
  margin: 20px;
  padding: 10px 20px;
}
```

- `margin`: The space I leave outside the border.
- `padding`: The space I leave inside the border.

Shorthand values follow this order:

| Values | Meaning |
|---|---|
| `10px` | All four sides |
| `10px 20px` | Top/bottom, left/right |
| `10px 20px 30px` | Top, left/right, bottom |
| `10px 20px 30px 40px` | Top, right, bottom, left |

I can also set individual sides with properties such as `margin-top` and `padding-left`.

#### Important behavior

- I need to watch for vertical margins between block elements collapsing into one margin instead of adding together.
- Horizontal margins generally do not collapse.
- I can use `margin: 0 auto` to horizontally center a block with a constrained width when enough space is available.
- Padding cannot be negative, but margins can.
- I use margins for space between elements and padding for space inside an element.

### 3. Normal Flow

I think of **normal flow** as the browser's default layout behavior, before I reposition elements with CSS layout techniques.

By default, block elements stack vertically, while inline elements flow alongside text and other inline content.

I want to understand normal flow before I rely on Flexbox, Grid, or positioning.

### 4. Block vs. Inline Elements

#### Block elements

Block elements typically begin on a new line and occupy the available width of their containing block.

Examples: `<div>`, `<p>`, `<h1>`, `<section>`.

- In normal flow, they stack vertically.
- `width` and `height` generally apply.
- Padding, borders, and margins affect their boxes.

#### Inline elements

Inline elements flow within a line of text and occupy only the space their content requires.

Examples: `<span>`, `<a>`, `<strong>`, `<em>`.

- They do not normally start a new line.
- `width` and `height` do not apply in the usual way.
- Horizontal padding, borders, and margins work.
- Vertical padding and borders can extend beyond the line box; vertical margins do not change line spacing in the usual way.

#### Inline-block elements

Inline-block elements flow inline but allow block-like sizing.

```css
.item {
  display: inline-block;
  width: 150px;
  height: 100px;
}
```

- They sit alongside other inline-level content.
- Allow explicit `width` and `height`.
- Support padding, borders, and margins like block boxes.

For more complex layouts, I may find Flexbox or Grid a better choice.

### 5. `<div>` vs. `<span>`

I can use either as a generic container; neither has inherent semantic meaning.

- `<div>`: A block-level container I can use to group sections or components.
- `<span>`: An inline container I can use to target part of a sentence.

```html
<div class="card">
  <h2>Learning CSS</h2>
  <p>Understand the <span class="highlight">box model</span>.</p>
</div>
```

I should use semantic HTML elements when they accurately describe the content, and use `div` or `span` when a generic container is appropriate.

**Reminder to myself:** Before I try a more advanced layout, I should check how an element's dimensions are calculated and how it participates in normal flow.
