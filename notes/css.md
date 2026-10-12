---
layout: page
title: Intro to CSS
description: >
  Notes I keep for myself while learning CSS.
hide_description: true
sitemap: false
---

0. this unordered seed list will be replaced by toc as unordered list
{:toc}

## What I Have Learned About CSS

I use **CSS (Cascading Style Sheets)** to control how HTML elements look and are laid out, including their colors, typography, and sizes.

### 1. Adding CSS to HTML

I can keep styling separate from HTML and share one stylesheet across multiple pages:

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

I have also seen these methods:
- **Internal CSS:** CSS inside `<style>` tags in the HTML `<head>`.
- **Inline CSS:** CSS inside an element's `style` attribute. I should avoid relying on it because it can create repetition and make maintenance harder.

### 2. CSS Syntax

I write a CSS rule with a selector and one or more declarations.

```css
selector {
  property: value;
}
```

- **Selector:** identifies the HTML elements I want to style.
- **Property:** tells CSS what I want to change.
- **Value:** tells CSS how I want to change it.
- I end each declaration with a semicolon and enclose declarations in curly braces.

### 3. CSS Selectors I Use

| Selector | Example | Selects |
|---|---|---|
| Universal | `*` | All elements |
| Type | `p` | All `<p>` elements |
| Class | `.notice` | Elements with `class="notice"` |
| ID | `#title` | The element with `id="title"` |
| Grouping | `h1, p` | All `<h1>` and `<p>` elements |
| Chaining | `.notice.urgent` | Elements with both classes |
| Descendant | `.card p` | `<p>` elements anywhere inside `.card` |

#### Class and ID reminders

- I can reuse classes across elements, and an element can have more than one class.
- I separate multiple classes with spaces in HTML: `class="notice urgent"`.
- I use hyphens in multiword class names, such as `.alert-text`.
- I should keep IDs unique within a page.
- Class and ID selectors are case-sensitive.
- In these basic selector forms, class and ID names cannot begin with a digit.
- I usually use classes for reusable styling and IDs for unique identifiers.

#### Grouping vs. chaining vs. descendants

```css
h1, p {
  color: navy;
}
```
**Grouping:** I can apply the same rule to different selectors.

```css
.notice.urgent {
  color: red;
}
```
**Chaining:** I select one element that matches both classes. There is no space between selectors.

```css
.card p {
  color: gray;
}
```
**Descendant combinator:** I select paragraphs nested anywhere inside an element with the `card` class. The space indicates the ancestor-descendant relationship.

### 4. CSS Properties I Keep Coming Back To

#### Colors

```css
p {
  color: #1100ff;
  background-color: transparent;
}
```

- `color`: the text color.
- `background-color`: the background color.
- I can write color values as names, HEX, RGB, or HSL.

#### Typography

```css
body {
  font-family: Arial, Helvetica, sans-serif;
  font-size: 16px;
  font-weight: 400;
  text-align: left;
}
```

- `font-family`: the font and fallback fonts I list in order of preference.
- `font-size`: the text size.
- `font-weight`: text thickness; `400` is normal and `700` is typically bold.
- `text-align`: horizontal alignment, such as `left`, `center`, or `right`.

#### Image dimensions

```css
img {
  width: 500px;
  height: auto;
}
```

When I set one dimension and use `auto` for the other, the image keeps its aspect ratio.

When practical, I should also add `width` and `height` attributes in HTML. They reserve space before an image loads and help prevent layout shifts.

### 5. Semantic HTML

I should choose elements for their meaning, such as `<h1>` for a main heading and `<p>` for a paragraph. I can use `<div>` when a generic container is appropriate.

I think of HTML as providing structure and meaning, and CSS as controlling presentation.

Source: [The Odin Project — Introduction to CSS](https://www.theodinproject.com/lessons/foundations-intro-to-css)

