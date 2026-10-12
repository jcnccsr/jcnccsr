---
layout: page
title: C Basics
description: >
  This chapter covers the flexbox, growing and shrinking, axes, and alignment.
hide_description: true
sitemap: false
---

0. this unordered seed list will be replaced by toc as unordered list
{:toc}

## Flexbox

**Flexbox** is a CSS layout model for arranging elements in a single dimension: a row or a column. Items can grow or shrink to fit available space.

A **flex container** is an element with `display: flex`. Its direct children become **flex items**.

```css
.container {
  display: flex;
}
```

By default, flex items are arranged horizontally in a row.

Flexbox properties belong either to the container or to its items. An element can be both a flex item and a flex container if it has its own children.

## Growing and Shrinking

The `flex` shorthand controls how a flex item grows, shrinks, and establishes its initial size.

```css
.item {
  flex: 1;
}
```

This commonly distributes available space equally among flex items.

The shorthand represents three properties:

- `flex-grow`: Determines how available positive space is distributed. Default: `0`.
- `flex-shrink`: Determines how items shrink when space is insufficient. Default: `1`.
- `flex-basis`: Sets the initial main-axis size before growing or shrinking. Default: `auto`.

```css
.item {
  flex: 1 1 200px;
  /* grow shrink basis */
}
```

### `flex-basis`

Defines an item's initial size along the main axis. With `flex-basis: auto`, the browser considers the item's main-size property, such as `width` in a row or `height` in a column.

### `flex-grow`

When positive free space exists, items grow according to their grow factors.

```css
.item-a { flex-grow: 1; }
.item-b { flex-grow: 2; }
```

Item B receives twice as much of the *distributable free space* as item A, assuming both can grow and other constraints do not interfere.

### `flex-shrink`

When items collectively exceed the available space, they shrink according to their shrink factors, adjusted by their flex base sizes.

```css
.item {
  flex-shrink: 1;
}
```

Setting `flex-shrink: 0` prevents an item from shrinking through this mechanism, potentially causing overflow.

### Common shorthand values

| Declaration | Meaning |
|---|---|
| `flex: 1` | Commonly expands to `1 1 0%` |
| `flex: auto` | Equivalent to `1 1 auto` |
| `flex: initial` | Equivalent to `0 1 auto` |
| `flex: none` | Equivalent to `0 0 auto` |

For predictable layouts, remember that `flex: 1` generally distributes space from a zero basis, while `flex: auto` considers the item's existing size.

## Flexbox Axes

Flexbox has two axes:

- **Main axis:** The direction in which flex items are laid out.
- **Cross axis:** Perpendicular to the main axis.

The `flex-direction` property determines the main axis.

```css
.container {
  display: flex;
  flex-direction: row;
}
```

| Value | Main axis |
|---|---|
| `row` | Horizontal, inline direction |
| `row-reverse` | Reverse horizontal direction |
| `column` | Vertical, block direction |
| `column-reverse` | Reverse vertical direction |

`row` is the default.

The axes are not permanently horizontal and vertical. When you change `flex-direction`, the main and cross axes change accordingly.

## Flexbox Alignment

Two essential properties control alignment inside a flex container:

- `justify-content`: Aligns and distributes items along the **main axis**.
- `align-items`: Aligns items along the **cross axis**.

```css
.container {
  display: flex;
  justify-content: center;
  align-items: center;
}
```

This centers items along both axes when the container has sufficient space.

### `justify-content`

Common values:

| Value | Behavior |
|---|---|
| `flex-start` | Items at the start |
| `flex-end` | Items at the end |
| `center` | Items centered |
| `space-between` | Equal space between items; none at the ends |
| `space-around` | Equal space around each item |
| `space-evenly` | Equal gaps between items and at both ends |

These properties distribute available space; they do not necessarily change item sizes.

### `align-items`

Common values:

| Value | Behavior |
|---|---|
| `stretch` | Stretches items across the cross axis when sizing permits |
| `flex-start` | Aligns items at the cross-axis start |
| `flex-end` | Aligns items at the cross-axis end |
| `center` | Centers items on the cross axis |
| `baseline` | Aligns text baselines |

`stretch` is the default. Explicit cross-axis sizing or other constraints can prevent stretching.

### `gap`

Adds space between flex items without adding space around the container's outer edges.

```css
.container {
  display: flex;
  gap: 16px;
  /* Or: row-gap: 8px; column-gap: 16px; */
}
```

Use `gap` for consistent spacing between items instead of adding margins to every item.

### Centering an element

```css
.container {
  display: flex;
  justify-content: center;
  align-items: center;
}
```

This centers the container's flex items along both axes, provided the container has sufficient dimensions. For a single item, `margin: auto` can also be useful in Flexbox.

## Practical Example

```html
<div class="container">
  <div class="item">One</div>
  <div class="item">Two</div>
  <div class="item">Three</div>
</div>
```

```css
.container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 16px;
  min-height: 200px;
  padding: 16px;
}

.item {
  flex: 1;
  padding: 20px;
  text-align: center;
  background-color: lightblue;
}
```

The container arranges its children in a row, aligns them vertically in the center, and adds spacing between them. Each item can grow to use available space.

**Core principle:** First identify the main axis and cross axis. Then decide whether you need to control item sizes (`flex`), alignment (`justify-content`, `align-items`), or spacing (`gap`).
