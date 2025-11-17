# CSS Cheat Sheet - Beginner to Pro

## Table of Contents
- [Basics](#basics)
- [Selectors](#selectors)
- [Box Model](#box-model)
- [Typography](#typography)
- [Colors & Backgrounds](#colors--backgrounds)
- [Layout - Display & Position](#layout---display--position)
- [Flexbox](#flexbox)
- [Grid](#grid)
- [Responsive Design](#responsive-design)
- [Transitions & Animations](#transitions--animations)
- [Transforms](#transforms)
- [Pseudo-classes & Pseudo-elements](#pseudo-classes--pseudo-elements)
- [Advanced Selectors](#advanced-selectors)
- [CSS Variables](#css-variables)
- [Modern CSS Features](#modern-css-features)
- [Best Practices](#best-practices)

---

## Basics

### CSS Syntax
```css
selector {
    property: value;
    another-property: value;
}
```

### Ways to Add CSS

#### Inline CSS
```html
<p style="color: blue; font-size: 16px;">Text</p>
```

#### Internal CSS
```html
<head>
    <style>
        p { color: blue; }
    </style>
</head>
```

#### External CSS
```html
<head>
    <link rel="stylesheet" href="styles.css">
</head>
```

### Comments
```css
/* This is a comment */

/* 
   Multi-line
   comment
*/
```

### Import
```css
@import url('other-styles.css');
@import 'mobile.css' screen and (max-width: 768px);
```

---

## Selectors

### Basic Selectors
```css
/* Universal selector */
* {
    margin: 0;
    padding: 0;
}

/* Element selector */
p {
    color: black;
}

/* Class selector */
.class-name {
    background: yellow;
}

/* ID selector */
#unique-id {
    font-weight: bold;
}

/* Multiple selectors */
h1, h2, h3 {
    font-family: Arial, sans-serif;
}
```

### Combinators
```css
/* Descendant selector (all descendants) */
div p {
    color: blue;
}

/* Child selector (direct children only) */
div > p {
    color: red;
}

/* Adjacent sibling selector (immediate next sibling) */
h1 + p {
    margin-top: 0;
}

/* General sibling selector (all following siblings) */
h1 ~ p {
    color: gray;
}
```

### Attribute Selectors
```css
/* Has attribute */
[href] {
    color: blue;
}

/* Exact match */
[type="text"] {
    border: 1px solid gray;
}

/* Contains word */
[class~="button"] {
    cursor: pointer;
}

/* Starts with */
[href^="https"] {
    color: green;
}

/* Ends with */
[href$=".pdf"] {
    background: url('pdf-icon.png');
}

/* Contains substring */
[href*="example"] {
    text-decoration: underline;
}

/* Case-insensitive */
[href*="example" i] {
    color: purple;
}
```

---

## Box Model

### Box Model Properties
```css
.box {
    /* Content */
    width: 300px;
    height: 200px;
    
    /* Padding (inside) */
    padding: 20px;
    padding-top: 10px;
    padding-right: 15px;
    padding-bottom: 10px;
    padding-left: 15px;
    padding: 10px 15px; /* vertical horizontal */
    padding: 10px 15px 20px 25px; /* top right bottom left */
    
    /* Border */
    border: 2px solid black;
    border-width: 2px;
    border-style: solid; /* solid, dashed, dotted, double, groove, ridge, inset, outset */
    border-color: black;
    border-radius: 10px;
    border-top-left-radius: 5px;
    
    /* Margin (outside) */
    margin: 20px;
    margin: 10px auto; /* center horizontally */
    margin: 10px 20px 30px 40px; /* top right bottom left */
    
    /* Box sizing */
    box-sizing: border-box; /* includes padding and border in width/height */
    box-sizing: content-box; /* default, excludes padding and border */
}
```

### Width & Height
```css
.element {
    width: 300px;
    height: 200px;
    
    min-width: 200px;
    max-width: 500px;
    
    min-height: 100px;
    max-height: 400px;
    
    /* Responsive units */
    width: 50%; /* relative to parent */
    width: 50vw; /* viewport width */
    width: 50vh; /* viewport height */
    width: 50vmin; /* smaller of vw or vh */
    width: 50vmax; /* larger of vw or vh */
}
```

### Overflow
```css
.container {
    overflow: visible; /* default, content overflows */
    overflow: hidden; /* clip overflow */
    overflow: scroll; /* always show scrollbar */
    overflow: auto; /* scrollbar when needed */
    
    overflow-x: hidden;
    overflow-y: auto;
    
    /* Text overflow */
    text-overflow: ellipsis;
    white-space: nowrap;
    overflow: hidden;
}
```

---

## Typography

### Font Properties
```css
.text {
    /* Font family */
    font-family: Arial, Helvetica, sans-serif;
    font-family: 'Georgia', serif;
    font-family: 'Courier New', monospace;
    
    /* Font size */
    font-size: 16px;
    font-size: 1rem; /* relative to root */
    font-size: 1.2em; /* relative to parent */
    font-size: clamp(1rem, 2vw, 2rem); /* responsive */
    
    /* Font weight */
    font-weight: normal; /* 400 */
    font-weight: bold; /* 700 */
    font-weight: 100; /* thin */
    font-weight: 900; /* black */
    
    /* Font style */
    font-style: normal;
    font-style: italic;
    font-style: oblique;
    
    /* Font variant */
    font-variant: small-caps;
    
    /* Line height */
    line-height: 1.5;
    line-height: 24px;
    line-height: 150%;
}
```

### Text Properties
```css
.text {
    /* Color */
    color: black;
    color: #333333;
    color: rgb(51, 51, 51);
    color: rgba(51, 51, 51, 0.8);
    color: hsl(0, 0%, 20%);
    
    /* Alignment */
    text-align: left;
    text-align: center;
    text-align: right;
    text-align: justify;
    
    /* Decoration */
    text-decoration: none;
    text-decoration: underline;
    text-decoration: overline;
    text-decoration: line-through;
    text-decoration: underline wavy red;
    
    /* Transform */
    text-transform: none;
    text-transform: uppercase;
    text-transform: lowercase;
    text-transform: capitalize;
    
    /* Spacing */
    letter-spacing: 0.1em;
    word-spacing: 0.2em;
    
    /* Shadow */
    text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
    text-shadow: 1px 1px 2px black, 0 0 1em red;
    
    /* Indent */
    text-indent: 2em;
    
    /* White space */
    white-space: normal;
    white-space: nowrap; /* no wrapping */
    white-space: pre; /* preserve spaces and line breaks */
    white-space: pre-wrap; /* preserve but wrap */
    
    /* Word break */
    word-break: normal;
    word-break: break-all;
    word-break: keep-all;
    
    /* Word wrap */
    word-wrap: break-word;
    overflow-wrap: break-word;
}
```

### Web Fonts
```css
/* Google Fonts */
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap');

/* Custom font */
@font-face {
    font-family: 'CustomFont';
    src: url('font.woff2') format('woff2'),
         url('font.woff') format('woff');
    font-weight: normal;
    font-style: normal;
    font-display: swap; /* improve performance */
}

.text {
    font-family: 'Roboto', sans-serif;
}
```

---

## Colors & Backgrounds

### Color Values
```css
.element {
    /* Named colors */
    color: red;
    
    /* Hexadecimal */
    color: #ff0000;
    color: #f00; /* shorthand */
    
    /* RGB */
    color: rgb(255, 0, 0);
    
    /* RGBA (with alpha/opacity) */
    color: rgba(255, 0, 0, 0.5);
    
    /* HSL (Hue, Saturation, Lightness) */
    color: hsl(0, 100%, 50%);
    
    /* HSLA */
    color: hsla(0, 100%, 50%, 0.5);
    
    /* Current color */
    border-color: currentColor;
}
```

### Background Properties
```css
.element {
    /* Background color */
    background-color: #f0f0f0;
    
    /* Background image */
    background-image: url('image.jpg');
    background-image: linear-gradient(to right, red, blue);
    
    /* Background repeat */
    background-repeat: no-repeat;
    background-repeat: repeat;
    background-repeat: repeat-x;
    background-repeat: repeat-y;
    
    /* Background position */
    background-position: center;
    background-position: top right;
    background-position: 50% 50%;
    background-position: 10px 20px;
    
    /* Background size */
    background-size: cover; /* cover entire element */
    background-size: contain; /* fit within element */
    background-size: 100% auto;
    background-size: 200px 100px;
    
    /* Background attachment */
    background-attachment: scroll;
    background-attachment: fixed; /* parallax effect */
    
    /* Background origin & clip */
    background-origin: border-box;
    background-origin: padding-box;
    background-origin: content-box;
    
    background-clip: border-box;
    background-clip: padding-box;
    background-clip: content-box;
    background-clip: text; /* clip to text */
    -webkit-background-clip: text;
    
    /* Shorthand */
    background: #f0f0f0 url('image.jpg') no-repeat center/cover;
    
    /* Multiple backgrounds */
    background-image: url('front.png'), url('back.jpg');
    background-position: center, top;
    background-repeat: no-repeat, repeat;
}
```

### Gradients
```css
.element {
    /* Linear gradient */
    background: linear-gradient(to right, red, blue);
    background: linear-gradient(45deg, red, yellow, blue);
    background: linear-gradient(to bottom, red 0%, yellow 50%, blue 100%);
    
    /* Radial gradient */
    background: radial-gradient(circle, red, blue);
    background: radial-gradient(ellipse at center, red, blue);
    
    /* Conic gradient */
    background: conic-gradient(red, yellow, green, blue, red);
    
    /* Repeating gradients */
    background: repeating-linear-gradient(45deg, red 0px, red 10px, blue 10px, blue 20px);
    background: repeating-radial-gradient(circle, red 0px, red 10px, blue 10px, blue 20px);
}
```

### Opacity
```css
.element {
    opacity: 1; /* fully opaque */
    opacity: 0.5; /* 50% transparent */
    opacity: 0; /* fully transparent */
}
```

---

## Layout - Display & Position

### Display Property
```css
.element {
    display: block; /* takes full width */
    display: inline; /* flows with text */
    display: inline-block; /* inline but can have width/height */
    display: none; /* removes from layout */
    display: flex; /* flexbox container */
    display: grid; /* grid container */
    display: inline-flex;
    display: inline-grid;
    
    /* Hide but keep space */
    visibility: hidden;
    visibility: visible;
}
```

### Position Property
```css
/* Static (default) */
.element {
    position: static;
}

/* Relative - positioned relative to normal position */
.element {
    position: relative;
    top: 10px;
    left: 20px;
    right: 10px;
    bottom: 5px;
}

/* Absolute - positioned relative to nearest positioned ancestor */
.element {
    position: absolute;
    top: 0;
    right: 0;
}

/* Fixed - positioned relative to viewport */
.element {
    position: fixed;
    bottom: 20px;
    right: 20px;
}

/* Sticky - toggles between relative and fixed */
.element {
    position: sticky;
    top: 0; /* sticks when scrolled to top */
}

/* Z-index (stacking order) */
.element {
    z-index: 1;
    z-index: 999;
    z-index: -1;
}
```

### Float & Clear
```css
.element {
    float: left;
    float: right;
    float: none;
    
    clear: left;
    clear: right;
    clear: both;
    clear: none;
}

/* Clearfix */
.clearfix::after {
    content: "";
    display: table;
    clear: both;
}
```

---

## Flexbox

### Flex Container
```css
.container {
    display: flex;
    
    /* Direction */
    flex-direction: row; /* default, left to right */
    flex-direction: row-reverse;
    flex-direction: column; /* top to bottom */
    flex-direction: column-reverse;
    
    /* Wrap */
    flex-wrap: nowrap; /* default, single line */
    flex-wrap: wrap; /* multiple lines */
    flex-wrap: wrap-reverse;
    
    /* Shorthand: direction + wrap */
    flex-flow: row wrap;
    
    /* Justify content (main axis) */
    justify-content: flex-start; /* default */
    justify-content: flex-end;
    justify-content: center;
    justify-content: space-between;
    justify-content: space-around;
    justify-content: space-evenly;
    
    /* Align items (cross axis) */
    align-items: stretch; /* default */
    align-items: flex-start;
    align-items: flex-end;
    align-items: center;
    align-items: baseline;
    
    /* Align content (multi-line) */
    align-content: flex-start;
    align-content: flex-end;
    align-content: center;
    align-content: space-between;
    align-content: space-around;
    align-content: stretch;
    
    /* Gap */
    gap: 20px;
    row-gap: 20px;
    column-gap: 10px;
}
```

### Flex Items
```css
.item {
    /* Order */
    order: 0; /* default */
    order: 1;
    order: -1;
    
    /* Flex grow (expand) */
    flex-grow: 0; /* default */
    flex-grow: 1; /* grow to fill space */
    
    /* Flex shrink (compress) */
    flex-shrink: 1; /* default */
    flex-shrink: 0; /* don't shrink */
    
    /* Flex basis (initial size) */
    flex-basis: auto; /* default */
    flex-basis: 200px;
    flex-basis: 50%;
    
    /* Shorthand: grow shrink basis */
    flex: 0 1 auto; /* default */
    flex: 1; /* grow and shrink equally */
    flex: 2; /* grow/shrink twice as much */
    flex: 0 0 200px; /* fixed width */
    
    /* Align self (override align-items) */
    align-self: auto;
    align-self: flex-start;
    align-self: flex-end;
    align-self: center;
    align-self: baseline;
    align-self: stretch;
}
```

### Flexbox Patterns
```css
/* Center content */
.center {
    display: flex;
    justify-content: center;
    align-items: center;
}

/* Space between navigation */
.nav {
    display: flex;
    justify-content: space-between;
}

/* Equal width columns */
.columns {
    display: flex;
}
.columns > * {
    flex: 1;
}

/* Holy grail layout */
.container {
    display: flex;
    min-height: 100vh;
    flex-direction: column;
}
.main {
    display: flex;
    flex: 1;
}
.sidebar {
    flex: 0 0 200px;
}
.content {
    flex: 1;
}
```

---

## Grid

### Grid Container
```css
.container {
    display: grid;
    
    /* Define columns */
    grid-template-columns: 200px 200px 200px;
    grid-template-columns: 1fr 1fr 1fr; /* fractional units */
    grid-template-columns: repeat(3, 1fr);
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    grid-template-columns: 200px auto 200px;
    
    /* Define rows */
    grid-template-rows: 100px 200px 100px;
    grid-template-rows: repeat(3, 100px);
    grid-template-rows: auto 1fr auto;
    
    /* Gap */
    gap: 20px;
    row-gap: 20px;
    column-gap: 10px;
    
    /* Justify items (horizontal) */
    justify-items: start;
    justify-items: end;
    justify-items: center;
    justify-items: stretch; /* default */
    
    /* Align items (vertical) */
    align-items: start;
    align-items: end;
    align-items: center;
    align-items: stretch; /* default */
    
    /* Justify content (grid within container) */
    justify-content: start;
    justify-content: end;
    justify-content: center;
    justify-content: stretch;
    justify-content: space-around;
    justify-content: space-between;
    justify-content: space-evenly;
    
    /* Align content */
    align-content: start;
    align-content: end;
    align-content: center;
    align-content: stretch;
    align-content: space-around;
    align-content: space-between;
    align-content: space-evenly;
    
    /* Shorthand */
    place-items: center; /* align-items + justify-items */
    place-content: center; /* align-content + justify-content */
}
```

### Grid Items
```css
.item {
    /* Column placement */
    grid-column-start: 1;
    grid-column-end: 3;
    grid-column: 1 / 3; /* shorthand */
    grid-column: 1 / span 2; /* span 2 columns */
    grid-column: 1 / -1; /* span to end */
    
    /* Row placement */
    grid-row-start: 1;
    grid-row-end: 3;
    grid-row: 1 / 3;
    grid-row: 1 / span 2;
    
    /* Area shorthand */
    grid-area: 1 / 1 / 3 / 3; /* row-start / col-start / row-end / col-end */
    
    /* Justify/Align self */
    justify-self: start;
    align-self: end;
    place-self: center; /* both */
}
```

### Grid Template Areas
```css
.container {
    display: grid;
    grid-template-areas:
        "header header header"
        "sidebar content content"
        "footer footer footer";
    grid-template-columns: 200px 1fr 1fr;
    grid-template-rows: auto 1fr auto;
    gap: 20px;
}

.header {
    grid-area: header;
}

.sidebar {
    grid-area: sidebar;
}

.content {
    grid-area: content;
}

.footer {
    grid-area: footer;
}
```

### Grid Patterns
```css
/* Responsive grid */
.grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
    gap: 20px;
}

/* 12-column grid system */
.grid-12 {
    display: grid;
    grid-template-columns: repeat(12, 1fr);
    gap: 20px;
}
.span-6 {
    grid-column: span 6;
}

/* Card grid */
.cards {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
    gap: 2rem;
}

/* Masonry-like layout */
.masonry {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    grid-auto-rows: 10px;
    gap: 10px;
}
.masonry-item {
    grid-row: span var(--row-span);
}
```

---

## Responsive Design

### Media Queries
```css
/* Basic syntax */
@media media-type and (condition) {
    /* CSS rules */
}

/* Common breakpoints */
/* Mobile first */
@media (min-width: 640px) {
    /* Tablet and up */
}

@media (min-width: 768px) {
    /* Tablet and up */
}

@media (min-width: 1024px) {
    /* Desktop and up */
}

@media (min-width: 1280px) {
    /* Large desktop and up */
}

/* Desktop first */
@media (max-width: 1279px) {
    /* Below large desktop */
}

@media (max-width: 1023px) {
    /* Below desktop */
}

@media (max-width: 767px) {
    /* Mobile */
}

/* Range */
@media (min-width: 768px) and (max-width: 1023px) {
    /* Tablet only */
}

/* Orientation */
@media (orientation: portrait) {
    /* Portrait mode */
}

@media (orientation: landscape) {
    /* Landscape mode */
}

/* Pixel density */
@media (-webkit-min-device-pixel-ratio: 2),
       (min-resolution: 192dpi) {
    /* Retina displays */
}

/* Dark mode */
@media (prefers-color-scheme: dark) {
    /* Dark mode styles */
}

/* Light mode */
@media (prefers-color-scheme: light) {
    /* Light mode styles */
}

/* Reduced motion */
@media (prefers-reduced-motion: reduce) {
    * {
        animation: none !important;
        transition: none !important;
    }
}

/* Print styles */
@media print {
    /* Printing styles */
}
```

### Container Queries
```css
/* Container query */
.container {
    container-type: inline-size;
    container-name: sidebar;
}

@container sidebar (min-width: 400px) {
    .card {
        display: grid;
        grid-template-columns: 1fr 2fr;
    }
}

/* Size-based */
@container (min-width: 500px) {
    .element {
        font-size: 2rem;
    }
}
```

### Responsive Units
```css
.element {
    /* Viewport units */
    width: 100vw; /* viewport width */
    height: 100vh; /* viewport height */
    font-size: 5vmin; /* smaller of vw/vh */
    font-size: 5vmax; /* larger of vw/vh */
    
    /* Relative units */
    font-size: 1rem; /* relative to root element */
    width: 2em; /* relative to font-size */
    padding: 1ch; /* width of "0" character */
    width: 50%; /* relative to parent */
    
    /* Clamp (responsive) */
    font-size: clamp(1rem, 2.5vw, 2rem);
    width: clamp(200px, 50%, 800px);
    
    /* Min/Max */
    width: min(500px, 100%);
    width: max(200px, 50%);
    
    /* Calc */
    width: calc(100% - 40px);
    height: calc(100vh - 60px);
}
```

---

## Transitions & Animations

### Transitions
```css
.element {
    /* Transition property */
    transition-property: all;
    transition-property: background-color;
    transition-property: transform, opacity;
    
    /* Transition duration */
    transition-duration: 0.3s;
    transition-duration: 300ms;
    
    /* Transition timing function */
    transition-timing-function: ease; /* default */
    transition-timing-function: linear;
    transition-timing-function: ease-in;
    transition-timing-function: ease-out;
    transition-timing-function: ease-in-out;
    transition-timing-function: cubic-bezier(0.42, 0, 0.58, 1);
    
    /* Transition delay */
    transition-delay: 0s;
    transition-delay: 0.2s;
    
    /* Shorthand */
    transition: all 0.3s ease;
    transition: background-color 0.3s, transform 0.5s;
    transition: transform 0.3s cubic-bezier(0.42, 0, 0.58, 1);
}

/* Example */
.button {
    background-color: blue;
    transform: scale(1);
    transition: all 0.3s ease;
}

.button:hover {
    background-color: darkblue;
    transform: scale(1.1);
}
```

### Animations
```css
/* Define keyframes */
@keyframes slide-in {
    from {
        transform: translateX(-100%);
        opacity: 0;
    }
    to {
        transform: translateX(0);
        opacity: 1;
    }
}

/* Multiple steps */
@keyframes pulse {
    0% {
        transform: scale(1);
    }
    50% {
        transform: scale(1.1);
    }
    100% {
        transform: scale(1);
    }
}

/* Apply animation */
.element {
    /* Animation name */
    animation-name: slide-in;
    
    /* Animation duration */
    animation-duration: 1s;
    
    /* Animation timing function */
    animation-timing-function: ease;
    
    /* Animation delay */
    animation-delay: 0.5s;
    
    /* Animation iteration count */
    animation-iteration-count: 1;
    animation-iteration-count: infinite;
    animation-iteration-count: 3;
    
    /* Animation direction */
    animation-direction: normal;
    animation-direction: reverse;
    animation-direction: alternate; /* back and forth */
    animation-direction: alternate-reverse;
    
    /* Animation fill mode */
    animation-fill-mode: none;
    animation-fill-mode: forwards; /* keep final state */
    animation-fill-mode: backwards; /* apply first state during delay */
    animation-fill-mode: both;
    
    /* Animation play state */
    animation-play-state: running;
    animation-play-state: paused;
    
    /* Shorthand */
    animation: slide-in 1s ease 0.5s infinite alternate forwards;
    animation: pulse 2s ease-in-out infinite;
}
```

### Common Animation Examples
```css
/* Fade in */
@keyframes fade-in {
    from { opacity: 0; }
    to { opacity: 1; }
}

/* Slide up */
@keyframes slide-up {
    from {
        transform: translateY(100%);
        opacity: 0;
    }
    to {
        transform: translateY(0);
        opacity: 1;
    }
}

/* Rotate */
@keyframes rotate {
    from { transform: rotate(0deg); }
    to { transform: rotate(360deg); }
}

/* Bounce */
@keyframes bounce {
    0%, 100% { transform: translateY(0); }
    50% { transform: translateY(-20px); }
}

/* Shake */
@keyframes shake {
    0%, 100% { transform: translateX(0); }
    10%, 30%, 50%, 70%, 90% { transform: translateX(-10px); }
    20%, 40%, 60%, 80% { transform: translateX(10px); }
}
```

---

## Transforms

### 2D Transforms
```css
.element {
    /* Translate (move) */
    transform: translate(50px, 100px);
    transform: translateX(50px);
    transform: translateY(100px);
    
    /* Scale (resize) */
    transform: scale(1.5);
    transform: scale(2, 0.5); /* x, y */
    transform: scaleX(2);
    transform: scaleY(0.5);
    
    /* Rotate */
    transform: rotate(45deg);
    transform: rotate(-90deg);
    
    /* Skew */
    transform: skew(10deg, 20deg);
    transform: skewX(10deg);
    transform: skewY(20deg);
    
    /* Multiple transforms */
    transform: translate(50px, 100px) rotate(45deg) scale(1.5);
    
    /* Transform origin (pivot point) */
    transform-origin: center; /* default */
    transform-origin: top left;
    transform-origin: 50% 50%;
    transform-origin: 20px 40px;
}
```

### 3D Transforms
```css
.element {
    /* Perspective (for 3D) */
    perspective: 1000px;
    
    /* Transform style */
    transform-style: preserve-3d;
    transform-style: flat;
    
    /* Backface visibility */
    backface-visibility: visible;
    backface-visibility: hidden;
    
    /* 3D translate */
    transform: translate3d(50px, 100px, 75px);
    transform: translateZ(100px);
    
    /* 3D rotate */
    transform: rotateX(45deg);
    transform: rotateY(45deg);
    transform: rotateZ(45deg);
    transform: rotate3d(1, 1, 1, 45deg);
    
    /* 3D scale */
    transform: scale3d(2, 2, 2);
    transform: scaleZ(2);
    
    /* Perspective origin */
    perspective-origin: center;
    perspective-origin: 50% 50%;
}
```

### Transform Examples
```css
/* Hover lift effect */
.card {
    transition: transform 0.3s;
}
.card:hover {
    transform: translateY(-10px);
}

/* Flip card */
.flip-container {
    perspective: 1000px;
}
.flipper {
    transform-style: preserve-3d;
    transition: transform 0.6s;
}
.flip-container:hover .flipper {
    transform: rotateY(180deg);
}
.front, .back {
    backface-visibility: hidden;
}
.back {
    transform: rotateY(180deg);
}

/* 3D cube */
.cube {
    transform-style: preserve-3d;
    transform: rotateX(-30deg) rotateY(45deg);
}
```

---

## Pseudo-classes & Pseudo-elements

### Pseudo-classes
```css
/* Link states */
a:link { color: blue; }
a:visited { color: purple; }
a:hover { color: red; }
a:active { color: orange; }

/* Focus */
input:focus { outline: 2px solid blue; }
button:focus-visible { outline: 2px solid blue; }

/* Form states */
input:disabled { opacity: 0.5; }
input:enabled { opacity: 1; }
input:checked { background: green; }
input:valid { border-color: green; }
input:invalid { border-color: red; }
input:required { border-color: blue; }
input:optional { border-color: gray; }
input:read-only { background: #f0f0f0; }
input:read-write { background: white; }

/* Structural */
:root { /* html element */ }
:empty { /* no children */ }
p:first-child { /* first child */ }
p:last-child { /* last child */ }
p:nth-child(2) { /* 2nd child */ }
p:nth-child(odd) { /* odd children */ }
p:nth-child(even) { /* even children */ }
p:nth-child(3n) { /* every 3rd */ }
p:nth-child(3n+1) { /* 1st, 4th, 7th... */ }
p:nth-last-child(2) { /* 2nd from end */ }
p:first-of-type { /* first of type */ }
p:last-of-type { /* last of type */ }
p:nth-of-type(2) { /* 2nd of type */ }
p:only-child { /* only child */ }
p:only-of-type { /* only of type */ }

/* Negation */
:not(p) { /* not paragraph */ }
:not(.class) { /* not this class */ }

/* Target */
:target { /* URL fragment target */ }

/* Others */
:hover { /* mouse over */ }
:active { /* being clicked */ }
:focus { /* has focus */ }
:focus-within { /* contains focused element */ }
:is(h1, h2, h3) { /* matches any */ }
:where(h1, h2, h3) { /* like :is but 0 specificity */ }
:has(img) { /* contains image */ }
```

### Pseudo-elements
```css
/* Before & After */
.element::before {
    content: "★ ";
    color: gold;
}

.element::after {
    content: " ✓";
    color: green;
}

/* Content values */
::before {
    content: "text";
    content: attr(data-value);
    content: url('icon.png');
    content: counter(my-counter);
    content: "";
}

/* First letter & line */
p::first-letter {
    font-size: 2em;
    font-weight: bold;
}

p::first-line {
    font-variant: small-caps;
}

/* Selection */
::selection {
    background: yellow;
    color: black;
}

/* Placeholder */
input::placeholder {
    color: #999;
    opacity: 1;
}

/* Marker (list bullets) */
li::marker {
    color: red;
    font-size: 1.5em;
}

/* Backdrop (for dialogs) */
dialog::backdrop {
    background: rgba(0, 0, 0, 0.5);
}
```

---

## Advanced Selectors

### Specificity
```
Inline style: 1000
ID: 100
Class/Attribute/Pseudo-class: 10
Element/Pseudo-element: 1

Example:
#id .class div = 100 + 10 + 1 = 111
```

### Combinator Examples
```css
/* All paragraphs inside article */
article p { }

/* Direct children paragraphs */
article > p { }

/* Paragraph immediately after h2 */
h2 + p { }

/* All paragraphs after h2 */
h2 ~ p { }

/* Complex combinations */
.container > .item:nth-child(2n) { }
nav ul li:not(:last-child) { }
form input[type="text"]:focus { }
```

---

## CSS Variables

### Defining Variables
```css
:root {
    /* Define global variables */
    --primary-color: #007bff;
    --secondary-color: #6c757d;
    --font-size-base: 16px;
    --spacing-unit: 8px;
    --border-radius: 4px;
}

.component {
    /* Define scoped variables */
    --local-color: #ff0000;
}
```

### Using Variables
```css
.element {
    color: var(--primary-color);
    font-size: var(--font-size-base);
    padding: var(--spacing-unit);
    
    /* Fallback value */
    color: var(--undefined-color, #000);
    
    /* Nested variables */
    --spacing: var(--spacing-unit);
    margin: calc(var(--spacing) * 2);
}
```

### Dynamic Variables
```css
:root {
    --primary-hue: 200;
    --primary-sat: 70%;
    --primary-light: 50%;
    --primary-color: hsl(var(--primary-hue), var(--primary-sat), var(--primary-light));
}

/* Change via JavaScript */
/* document.documentElement.style.setProperty('--primary-hue', '300'); */
```

### Theme Example
```css
:root {
    --bg-color: #ffffff;
    --text-color: #000000;
}

[data-theme="dark"] {
    --bg-color: #000000;
    --text-color: #ffffff;
}

body {
    background-color: var(--bg-color);
    color: var(--text-color);
}
```

---

## Modern CSS Features

### Aspect Ratio
```css
.element {
    aspect-ratio: 16 / 9;
    aspect-ratio: 1; /* square */
    aspect-ratio: 4 / 3;
}
```

### Object Fit & Position
```css
img {
    object-fit: cover; /* crop to fill */
    object-fit: contain; /* scale to fit */
    object-fit: fill; /* stretch */
    object-fit: none; /* original size */
    object-fit: scale-down;
    
    object-position: center;
    object-position: top right;
    object-position: 50% 50%;
}
```

### Filters
```css
img {
    filter: blur(5px);
    filter: brightness(1.5);
    filter: contrast(200%);
    filter: grayscale(100%);
    filter: hue-rotate(90deg);
    filter: invert(100%);
    filter: opacity(50%);
    filter: saturate(200%);
    filter: sepia(100%);
    filter: drop-shadow(2px 2px 4px rgba(0,0,0,0.5));
    
    /* Multiple filters */
    filter: brightness(1.2) contrast(1.1) saturate(1.3);
}

/* Backdrop filter (blur background) */
.glassmorphism {
    background: rgba(255, 255, 255, 0.1);
    backdrop-filter: blur(10px);
}
```

### Blend Modes
```css
.element {
    mix-blend-mode: multiply;
    mix-blend-mode: screen;
    mix-blend-mode: overlay;
    mix-blend-mode: darken;
    mix-blend-mode: lighten;
    mix-blend-mode: color-dodge;
    mix-blend-mode: color-burn;
    mix-blend-mode: difference;
    mix-blend-mode: exclusion;
    
    background-blend-mode: multiply;
}
```

### Clip Path
```css
.element {
    clip-path: circle(50%);
    clip-path: ellipse(50% 30%);
    clip-path: polygon(50% 0%, 100% 50%, 50% 100%, 0% 50%);
    clip-path: inset(10px 20px 30px 40px);
    clip-path: path('M 0 0 L 100 0 L 100 100 Z');
}
```

### Scroll Behavior
```css
html {
    scroll-behavior: smooth;
}

.container {
    scroll-snap-type: y mandatory;
    scroll-snap-type: x proximity;
    overflow-y: scroll;
}

.item {
    scroll-snap-align: start;
    scroll-snap-align: center;
    scroll-snap-align: end;
}

/* Scroll padding */
.container {
    scroll-padding: 20px;
}
```

### CSS Grid Advanced
```css
/* Subgrid */
.container {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
}
.item {
    display: grid;
    grid-template-columns: subgrid;
}
```

### Logical Properties
```css
.element {
    /* Instead of left/right, use start/end */
    margin-inline-start: 20px; /* left in LTR, right in RTL */
    margin-inline-end: 20px;
    margin-block-start: 10px; /* top */
    margin-block-end: 10px; /* bottom */
    
    padding-inline: 20px; /* horizontal */
    padding-block: 10px; /* vertical */
    
    border-inline-start: 1px solid black;
    
    inset-inline-start: 0; /* left/right */
    inset-block-start: 0; /* top/bottom */
}
```

### Custom Scrollbars
```css
/* Webkit browsers */
::-webkit-scrollbar {
    width: 10px;
}

::-webkit-scrollbar-track {
    background: #f1f1f1;
}

::-webkit-scrollbar-thumb {
    background: #888;
    border-radius: 5px;
}

::-webkit-scrollbar-thumb:hover {
    background: #555;
}

/* Firefox */
* {
    scrollbar-width: thin;
    scrollbar-color: #888 #f1f1f1;
}
```

---

## Best Practices

### Organization
```css
/* Use consistent formatting */
.element {
    /* Positioning */
    position: relative;
    top: 0;
    left: 0;
    z-index: 1;
    
    /* Display & Box Model */
    display: flex;
    width: 100%;
    height: auto;
    margin: 0;
    padding: 20px;
    
    /* Typography */
    font-family: Arial, sans-serif;
    font-size: 16px;
    line-height: 1.5;
    text-align: center;
    
    /* Visual */
    background: white;
    border: 1px solid black;
    border-radius: 4px;
    box-shadow: 0 2px 4px rgba(0,0,0,0.1);
    
    /* Misc */
    opacity: 1;
    cursor: pointer;
    transition: all 0.3s;
}
```

### Naming Conventions

#### BEM (Block Element Modifier)
```css
/* Block */
.card { }

/* Element */
.card__title { }
.card__description { }

/* Modifier */
.card--featured { }
.card__title--large { }
```

### Performance Tips
```css
/* Avoid expensive properties */
/* Bad */
.element {
    box-shadow: 0 0 10px rgba(0,0,0,0.5);
    filter: blur(5px);
}

/* Use transform instead of position */
/* Bad */
.element {
    position: relative;
    left: 100px;
}

/* Good */
.element {
    transform: translateX(100px);
}

/* Use will-change for animations */
.element {
    will-change: transform, opacity;
}

/* Remove will-change after animation */
.element.animated {
    animation: slide 1s;
}
.element.animation-done {
    will-change: auto;
}
```

### Accessibility
```css
/* Focus styles */
:focus-visible {
    outline: 2px solid blue;
    outline-offset: 2px;
}

/* Reduced motion */
@media (prefers-reduced-motion: reduce) {
    * {
        animation-duration: 0.01ms !important;
        animation-iteration-count: 1 !important;
        transition-duration: 0.01ms !important;
    }
}

/* Sufficient contrast */
.text {
    color: #333; /* Good contrast on white */
}

/* Screen reader only */
.sr-only {
    position: absolute;
    width: 1px;
    height: 1px;
    padding: 0;
    margin: -1px;
    overflow: hidden;
    clip: rect(0, 0, 0, 0);
    white-space: nowrap;
    border: 0;
}
```

### Useful Patterns
```css
/* Truncate text */
.truncate {
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
}

/* Multi-line truncate */
.truncate-multi {
    display: -webkit-box;
    -webkit-line-clamp: 3;
    -webkit-box-orient: vertical;
    overflow: hidden;
}

/* Aspect ratio box (legacy) */
.aspect-box {
    position: relative;
    width: 100%;
    padding-bottom: 56.25%; /* 16:9 */
}
.aspect-box > * {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
}

/* Clearfix */
.clearfix::after {
    content: "";
    display: table;
    clear: both;
}

/* Center absolutely */
.center-absolute {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
}

/* Overlay */
.overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.5);
    z-index: 1000;
}

/* Hide visually but keep for screen readers */
.visually-hidden {
    position: absolute;
    width: 1px;
    height: 1px;
    margin: -1px;
    padding: 0;
    overflow: hidden;
    clip: rect(0, 0, 0, 0);
    white-space: nowrap;
    border: 0;
}

/* Reset button styles */
.button-reset {
    background: none;
    border: none;
    padding: 0;
    margin: 0;
    font: inherit;
    color: inherit;
    cursor: pointer;
}

/* Reset list styles */
.list-reset {
    list-style: none;
    padding: 0;
    margin: 0;
}
```

---

**Pro Tip:** Master Flexbox and Grid for modern layouts, use CSS Variables for maintainability, and always consider performance and accessibility!