---
layout: page
title: Web Development Basics
description: >
  Web basics.
hide_description: true
sitemap: false
---

This chapter covers the basics of web development.

0. this unordered seed list will be replaced by toc as unordered list
{:toc}


## HTML

~~~html
<!-- file: "index.html" -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document Title</title>
    
    <link rel="stylesheet" href="/assets/css/style.css">
</head>
<body>

    <h1>Hello, World!</h1>

    <script src="/assets/js/main.js"></script>
</body>
</html>
~~~

## CSS

### CSS Reset

~~~css

/* Box sizing rules */
*,
*::before,
*::after {
  box-sizing: border-box;
}

/* Prevent font size inflation */
html {
  -moz-text-size-adjust: none;
  -webkit-text-size-adjust: none;
  text-size-adjust: none;
}

/* Remove default margin in favour of better control in authored CSS */
body, h1, h2, h3, h4, p,
figure, blockquote, dl, dd {
  margin-block-end: 0;
}

/* Remove list styles on ul, ol elements with a list role, which suggests default styling will be removed */
ul[role='list'],
ol[role='list'] {
  list-style: none;
}

/* Set core body defaults */
body {
  min-height: 100vh;
  line-height: 1.5;
}

/* Set shorter line heights on headings and interactive elements */
h1, h2, h3, h4,
button, input, label {
  line-height: 1.1;
}

/* Balance text wrapping on headings */
h1, h2,
h3, h4 {
  text-wrap: balance;
}

/* A elements that don't have a class get default styles */
a:not([class]) {
  text-decoration-skip-ink: auto;
  color: currentColor;
}

/* Make images easier to work with */
img,
picture {
  max-width: 100%;
  display: block;
}

/* Inherit fonts for inputs and buttons */
input, button,
textarea, select {
  font-family: inherit;
  font-size: inherit;
}

/* Make sure textareas without a rows attribute are not tiny */
textarea:not([rows]) {
  min-height: 10em;
}

/* Anything that has been anchored to should have extra scroll margin */
:target {
  scroll-margin-block: 5ex;
}
~~~

### Color Palette

~~~css
:root {
  --hue-mint: #3AB795;
  --hue-cela: #A0E8AF;
  --hue-teal: #86BAA1;
  --hue-sand: #EDEAD0;
  --hue-gold: #FFCF56;
}
~~~

## JS
~~~js
/*
*/
~~~