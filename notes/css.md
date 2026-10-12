---
layout: page
title: Intro to CSS
description: >
  This chapter covers the basics of CSS.
hide_description: true
sitemap: false
---

0. this unordered seed list will be replaced by toc as unordered list
{:toc}

## Introduction to CSS

**CSS (Cascading Style Sheets)** controls the presentation of HTML elements, including colors, typography, sizing, and layout.

### 1. Adding CSS to HTML

Keeps styling separate from HTML and allows multiple pages to share the same stylesheet.

```html
<!-- index.html -->
<head>
  <link rel="stylesheet" href="styles.css">
</head>
```

```css
/* styles.css */
p {
  color: blue;
}
```

Other methods:
- **Internal CSS:** CSS inside `<style>` tags in the HTML `<head>`.
- **Inline CSS:** CSS inside an element's `style` attribute. Generally avoid it because it creates repetition and makes maintenance harder.

### 2. CSS Syntax

A CSS rule consists of a selector and one or more declarations.

```css
selector {
  property: value;
}
```

- **Selector:** identifies which HTML elements to style.
- **Property:** specifies what to change.
- **Value:** specifies how to change it.
- Each declaration ends with a semicolon; declarations are enclosed in curly braces.

### 3. CSS Selectors

| Selector | Example | Selects |
|---|---|---|
| Universal | `*` | All elements |
| Type | `p` | All `<p>` elements |
| Class | `.notice` | Elements with `class="notice"` |
| ID | `#title` | The element with `id="title"` |
| Grouping | `h1, p` | All `<h1>` and `<p>` elements |
| Chaining | `.notice.urgent` | Elements with both classes |
| Descendant | `.card p` | `<p>` elements anywhere inside `.card` |

#### Class and ID rules

- Classes can be reused across multiple elements; an element can have multiple classes.
- Separate multiple classes with spaces in HTML: `class="notice urgent"`.
- Use hyphens for multiword class names: `.alert-text`.
- IDs should be unique within a page.
- Class and ID selectors are case-sensitive.
- Class and ID names cannot begin with a digit in these basic selector forms.
- Prefer classes for reusable styling; use IDs when a unique identifier is appropriate.

#### Grouping vs. chaining vs. descendants

```css
h1, p {
  color: navy;
}
```
**Grouping:** applies the same rule to different selectors.

```css
.notice.urgent {
  color: red;
}
```
**Chaining:** selects one element that matches both classes. No space between selectors.

```css
.card p {
  color: gray;
}
```
**Descendant combinator:** selects paragraphs nested anywhere inside an element with the `card` class. A space indicates the ancestor–descendant relationship.

### 4. Common CSS Properties

#### Colors

```css
p {
  color: #1100ff;
  background-color: transparent;
}
```

- `color`: text color.
- `background-color`: background color.
- Color values can use names, HEX, RGB, or HSL.

#### Typography

```css
body {
  font-family: Arial, Helvetica, sans-serif;
  font-size: 16px;
  font-weight: 400;
  text-align: left;
}
```

- `font-family`: font and fallback fonts, listed in order of preference.
- `font-size`: text size.
- `font-weight`: text thickness; `400` is normal and `700` is typically bold.
- `text-align`: horizontal alignment, such as `left`, `center`, or `right`.

#### Image dimensions

```css
img {
  width: 500px;
  height: auto;
}
```

Setting one dimension and using `auto` for the other preserves the image's aspect ratio.

Also specify `width` and `height` attributes in HTML when practical. These reserve space before an image loads and help prevent layout shifts.

### 5. Semantic HTML

Use elements according to their meaning, such as `<h1>` for a main heading and `<p>` for a paragraph. Use `<div>` when a generic container is appropriate.

HTML provides the structure and meaning; CSS controls presentation.

Source: [The Odin Project — Introduction to CSS](https://www.theodinproject.com/lessons/foundations-intro-to-css)

