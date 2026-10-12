---
layout: page
title: CSS Flexbox
description: >
  Notes I keep about Flexbox, sizing, axes, and alignment.
hide_description: true
sitemap: false
---

0. this unordered seed list will be replaced by toc as unordered list
{:toc}

## Flexbox

I use **Flexbox** to arrange elements in one dimension: a row or a column. Items can grow or shrink to fit the available space.

An element with `display: flex` is a **flex container**, and its direct children are **flex items**.

```css
.container {
  display: flex;
}
```

I need to remember that flex items are arranged horizontally in a row by default.

I set some Flexbox properties on the container and others on its items. An element with its own children can be both a flex item and a flex container.

## Growing and Shrinking

I use the `flex` shorthand to control how an item grows, shrinks, and gets its initial size.

```css
.item {
  flex: 1;
}
```

This commonly distributes available space equally among flex items.

The shorthand represents three properties I want to keep straight:

- `flex-grow`: Determines how I distribute extra space. Default: `0`.
- `flex-shrink`: Determines how items shrink when space is insufficient. Default: `1`.
- `flex-basis`: Sets the initial main-axis size before growing or shrinking. Default: `auto`.

```css
.item {
  flex: 1 1 200px;
  /* grow shrink basis */
}
```

### `flex-basis`

`flex-basis` defines an item's initial size along the main axis. With `flex-basis: auto`, the browser considers the item's main-size property, such as `width` in a row or `height` in a column.

### `flex-grow`

When positive free space exists, items grow according to their grow factors.

```css
.item-a { flex-grow: 1; }
.item-b { flex-grow: 2; }
```

Here, item B gets twice as much of the *distributable free space* as item A, assuming both can grow and other constraints do not interfere.

### `flex-shrink`

When my items collectively exceed the available space, they shrink according to their shrink factors, adjusted by their flex base sizes.

```css
.item {
  flex-shrink: 1;
}
```

If I set `flex-shrink: 0`, the item will not shrink through this mechanism, which can cause overflow.

### Common shorthand values

| Declaration | Meaning |
|---|---|
| `flex: 1` | Commonly expands to `1 1 0%` |
| `flex: auto` | Equivalent to `1 1 auto` |
| `flex: initial` | Equivalent to `0 1 auto` |
| `flex: none` | Equivalent to `0 0 auto` |

I want to remember that `flex: 1` generally distributes space from a zero basis, while `flex: auto` considers the item's existing size.

## Flexbox Axes

I need to keep the two Flexbox axes in mind:

- **Main axis:** The direction in which flex items are laid out.
- **Cross axis:** The direction perpendicular to the main axis.

I use `flex-direction` to set the main axis.

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

I should not assume the axes are always horizontal and vertical. When I change `flex-direction`, the main and cross axes change too.

## Flexbox Alignment

These two properties help me control alignment inside a flex container:

- `justify-content`: Aligns and distributes items along the **main axis**.
- `align-items`: Aligns items along the **cross axis**.

```css
.container {
  display: flex;
  justify-content: center;
  align-items: center;
}
```

This centers the items along both axes when the container has enough space.

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

I use these values to distribute available space; they do not necessarily change item sizes.

### `align-items`

Common values:

| Value | Behavior |
|---|---|
| `stretch` | Stretches items across the cross axis when sizing permits |
| `flex-start` | Aligns items at the cross-axis start |
| `flex-end` | Aligns items at the cross-axis end |
| `center` | Centers items on the cross axis |
| `baseline` | Aligns text baselines |

`stretch` is the default. Explicit cross-axis sizing or other constraints can prevent it from stretching.

### `gap`

I use `gap` to add space between flex items without adding space around the container's outer edges.

```css
.container {
  display: flex;
  gap: 16px;
  /* Or: row-gap: 8px; column-gap: 16px; */
}
```

I can use `gap` for consistent spacing between items instead of adding margins to each one.

### Centering items

```css
.container {
  display: flex;
  justify-content: center;
  align-items: center;
}
```

This centers the flex items along both axes if the container is large enough. For one item, I can also use `margin: auto` in Flexbox.

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

Here, the container arranges its children in a row, centers them vertically, and adds space between them. Each item can grow to use the available space.

**Reminder to myself:** First I identify the main and cross axes. Then I decide whether I need to control item sizes (`flex`), alignment (`justify-content`, `align-items`), or spacing (`gap`).
