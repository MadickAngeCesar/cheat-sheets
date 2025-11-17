# HTML Cheat Sheet - Beginner to Pro

## Table of Contents
- [Basics](#basics)
- [Document Structure](#document-structure)
- [Text Elements](#text-elements)
- [Links & Navigation](#links--navigation)
- [Lists](#lists)
- [Tables](#tables)
- [Forms](#forms)
- [Semantic HTML](#semantic-html)
- [Multimedia](#multimedia)
- [Meta Tags & SEO](#meta-tags--seo)
- [Accessibility](#accessibility)
- [Advanced Concepts](#advanced-concepts)

---

## Basics

### HTML Document Template
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Page Title</title>
</head>
<body>
    <!-- Content goes here -->
</body>
</html>
```

### Common Elements
```html
<!-- Headings (h1 - h6) -->
<h1>Main Heading</h1>
<h2>Sub Heading</h2>

<!-- Paragraph -->
<p>This is a paragraph.</p>

<!-- Line break -->
<br>

<!-- Horizontal rule -->
<hr>

<!-- Comments -->
<!-- This is a comment -->
```

---

## Document Structure

### Head Elements
```html
<head>
    <!-- Character encoding -->
    <meta charset="UTF-8">
    
    <!-- Viewport for responsive design -->
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    
    <!-- Page title -->
    <title>Page Title</title>
    
    <!-- External stylesheet -->
    <link rel="stylesheet" href="styles.css">
    
    <!-- Favicon -->
    <link rel="icon" type="image/png" href="favicon.png">
    
    <!-- External JavaScript -->
    <script src="script.js" defer></script>
    
    <!-- Internal styles -->
    <style>
        /* CSS here */
    </style>
</head>
```

---

## Text Elements

### Formatting
```html
<!-- Bold -->
<strong>Important text</strong> or <b>Bold text</b>

<!-- Italic -->
<em>Emphasized text</em> or <i>Italic text</i>

<!-- Underline -->
<u>Underlined text</u>

<!-- Strikethrough -->
<s>Strikethrough</s> or <del>Deleted text</del>

<!-- Subscript & Superscript -->
H<sub>2</sub>O
x<sup>2</sup>

<!-- Mark/Highlight -->
<mark>Highlighted text</mark>

<!-- Small text -->
<small>Small print</small>

<!-- Code -->
<code>inline code</code>
<pre><code>
    code block
</code></pre>

<!-- Blockquote -->
<blockquote cite="source">
    Quote text
</blockquote>

<!-- Abbreviation -->
<abbr title="HyperText Markup Language">HTML</abbr>
```

---

## Links & Navigation

### Basic Links
```html
<!-- External link -->
<a href="https://example.com">Visit Example</a>

<!-- Open in new tab -->
<a href="https://example.com" target="_blank" rel="noopener noreferrer">
    External Site
</a>

<!-- Email link -->
<a href="mailto:email@example.com">Send Email</a>

<!-- Phone link -->
<a href="tel:+1234567890">Call Us</a>

<!-- Download link -->
<a href="file.pdf" download>Download PDF</a>

<!-- Anchor link (same page) -->
<a href="#section-id">Jump to Section</a>

<!-- Relative links -->
<a href="./page.html">Same directory</a>
<a href="../page.html">Parent directory</a>
```

---

## Lists

### Unordered List
```html
<ul>
    <li>Item 1</li>
    <li>Item 2</li>
    <li>Item 3</li>
</ul>
```

### Ordered List
```html
<ol>
    <li>First</li>
    <li>Second</li>
    <li>Third</li>
</ol>

<!-- Custom start number -->
<ol start="5">
    <li>Fifth item</li>
</ol>

<!-- Reversed -->
<ol reversed>
    <li>Item</li>
</ol>
```

### Description List
```html
<dl>
    <dt>Term 1</dt>
    <dd>Description 1</dd>
    
    <dt>Term 2</dt>
    <dd>Description 2</dd>
</dl>
```

### Nested Lists
```html
<ul>
    <li>Item 1
        <ul>
            <li>Sub-item 1.1</li>
            <li>Sub-item 1.2</li>
        </ul>
    </li>
    <li>Item 2</li>
</ul>
```

---

## Tables

### Basic Table
```html
<table>
    <thead>
        <tr>
            <th>Header 1</th>
            <th>Header 2</th>
            <th>Header 3</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Data 1</td>
            <td>Data 2</td>
            <td>Data 3</td>
        </tr>
        <tr>
            <td>Data 4</td>
            <td>Data 5</td>
            <td>Data 6</td>
        </tr>
    </tbody>
    <tfoot>
        <tr>
            <td colspan="3">Footer</td>
        </tr>
    </tfoot>
</table>
```

### Advanced Table Features
```html
<!-- Column spanning -->
<td colspan="2">Spans 2 columns</td>

<!-- Row spanning -->
<td rowspan="2">Spans 2 rows</td>

<!-- Column groups -->
<table>
    <colgroup>
        <col style="background-color: #f0f0f0">
        <col span="2" style="background-color: #e0e0e0">
    </colgroup>
    <!-- table content -->
</table>

<!-- Caption -->
<table>
    <caption>Table Caption</caption>
    <!-- table content -->
</table>
```

---

## Forms

### Basic Form Structure
```html
<form action="/submit" method="POST">
    <!-- Form fields -->
    <button type="submit">Submit</button>
</form>
```

### Input Types
```html
<!-- Text input -->
<input type="text" name="username" placeholder="Enter username" required>

<!-- Password -->
<input type="password" name="password" minlength="8">

<!-- Email -->
<input type="email" name="email" placeholder="user@example.com">

<!-- Number -->
<input type="number" name="age" min="0" max="120" step="1">

<!-- Range -->
<input type="range" name="volume" min="0" max="100" value="50">

<!-- Date -->
<input type="date" name="birthdate">

<!-- Time -->
<input type="time" name="appointment">

<!-- Datetime-local -->
<input type="datetime-local" name="meeting">

<!-- Color -->
<input type="color" name="theme-color">

<!-- File -->
<input type="file" name="upload" accept=".jpg,.png,.pdf">

<!-- Checkbox -->
<input type="checkbox" name="subscribe" id="sub" checked>
<label for="sub">Subscribe to newsletter</label>

<!-- Radio buttons -->
<input type="radio" name="gender" value="male" id="male">
<label for="male">Male</label>
<input type="radio" name="gender" value="female" id="female">
<label for="female">Female</label>

<!-- Hidden -->
<input type="hidden" name="user-id" value="12345">
```

### Other Form Elements
```html
<!-- Textarea -->
<textarea name="message" rows="5" cols="30" placeholder="Your message"></textarea>

<!-- Select dropdown -->
<select name="country">
    <option value="">Select a country</option>
    <option value="us">United States</option>
    <option value="uk">United Kingdom</option>
    <option value="ca">Canada</option>
</select>

<!-- Multiple select -->
<select name="skills" multiple>
    <option value="html">HTML</option>
    <option value="css">CSS</option>
    <option value="js">JavaScript</option>
</select>

<!-- Option groups -->
<select name="food">
    <optgroup label="Fruits">
        <option value="apple">Apple</option>
        <option value="banana">Banana</option>
    </optgroup>
    <optgroup label="Vegetables">
        <option value="carrot">Carrot</option>
        <option value="broccoli">Broccoli</option>
    </optgroup>
</select>

<!-- Datalist (autocomplete) -->
<input list="browsers" name="browser">
<datalist id="browsers">
    <option value="Chrome">
    <option value="Firefox">
    <option value="Safari">
    <option value="Edge">
</datalist>

<!-- Fieldset & Legend -->
<fieldset>
    <legend>Personal Information</legend>
    <input type="text" name="firstname" placeholder="First Name">
    <input type="text" name="lastname" placeholder="Last Name">
</fieldset>

<!-- Label association -->
<label for="username">Username:</label>
<input type="text" id="username" name="username">

<!-- Button types -->
<button type="submit">Submit</button>
<button type="reset">Reset</button>
<button type="button">Click Me</button>
```

### Form Validation
```html
<!-- Required field -->
<input type="text" name="username" required>

<!-- Pattern validation -->
<input type="text" name="zipcode" pattern="[0-9]{5}" title="5-digit zip code">

<!-- Min/Max length -->
<input type="text" name="username" minlength="3" maxlength="20">

<!-- Min/Max value -->
<input type="number" name="age" min="18" max="100">

<!-- Custom validation message -->
<input type="email" name="email" required 
       oninvalid="this.setCustomValidity('Please enter a valid email')"
       oninput="this.setCustomValidity('')">
```

---

## Semantic HTML

### Layout Elements
```html
<!-- Header -->
<header>
    <nav>
        <a href="/">Home</a>
        <a href="/about">About</a>
    </nav>
</header>

<!-- Main content -->
<main>
    <!-- Article -->
    <article>
        <h2>Article Title</h2>
        <p>Article content...</p>
    </article>
    
    <!-- Section -->
    <section>
        <h2>Section Title</h2>
        <p>Section content...</p>
    </section>
    
    <!-- Aside -->
    <aside>
        <h3>Related Info</h3>
        <p>Sidebar content...</p>
    </aside>
</main>

<!-- Footer -->
<footer>
    <p>&copy; 2024 Company Name</p>
</footer>

<!-- Navigation -->
<nav>
    <ul>
        <li><a href="/">Home</a></li>
        <li><a href="/about">About</a></li>
    </ul>
</nav>

<!-- Figure with caption -->
<figure>
    <img src="image.jpg" alt="Description">
    <figcaption>Image caption</figcaption>
</figure>

<!-- Time element -->
<time datetime="2024-01-15">January 15, 2024</time>

<!-- Address -->
<address>
    Contact us at <a href="mailto:info@example.com">info@example.com</a>
</address>

<!-- Details/Summary (accordion) -->
<details>
    <summary>Click to expand</summary>
    <p>Hidden content here</p>
</details>
```

---

## Multimedia

### Images
```html
<!-- Basic image -->
<img src="image.jpg" alt="Description" width="300" height="200">

<!-- Responsive image -->
<img src="image.jpg" alt="Description" loading="lazy">

<!-- Picture element (responsive images) -->
<picture>
    <source media="(min-width: 800px)" srcset="large.jpg">
    <source media="(min-width: 400px)" srcset="medium.jpg">
    <img src="small.jpg" alt="Description">
</picture>

<!-- Image with srcset -->
<img src="image.jpg" 
     srcset="image-320w.jpg 320w,
             image-640w.jpg 640w,
             image-1280w.jpg 1280w"
     sizes="(max-width: 320px) 280px,
            (max-width: 640px) 600px,
            1200px"
     alt="Description">

<!-- Image map -->
<img src="map.jpg" usemap="#image-map" alt="Map">
<map name="image-map">
    <area shape="rect" coords="0,0,100,100" href="link1.html" alt="Area 1">
    <area shape="circle" coords="150,150,50" href="link2.html" alt="Area 2">
</map>
```

### Video
```html
<!-- Basic video -->
<video src="video.mp4" controls width="640" height="360">
    Your browser doesn't support video.
</video>

<!-- Video with multiple sources -->
<video controls width="640" height="360" poster="thumbnail.jpg">
    <source src="video.mp4" type="video/mp4">
    <source src="video.webm" type="video/webm">
    <source src="video.ogg" type="video/ogg">
    Your browser doesn't support video.
</video>

<!-- Video attributes -->
<video controls autoplay muted loop preload="auto">
    <source src="video.mp4" type="video/mp4">
</video>

<!-- Video with tracks (subtitles) -->
<video controls>
    <source src="video.mp4" type="video/mp4">
    <track src="subtitles-en.vtt" kind="subtitles" srclang="en" label="English">
    <track src="subtitles-es.vtt" kind="subtitles" srclang="es" label="Spanish">
</video>
```

### Audio
```html
<!-- Basic audio -->
<audio src="audio.mp3" controls>
    Your browser doesn't support audio.
</audio>

<!-- Audio with multiple sources -->
<audio controls>
    <source src="audio.mp3" type="audio/mpeg">
    <source src="audio.ogg" type="audio/ogg">
    <source src="audio.wav" type="audio/wav">
    Your browser doesn't support audio.
</audio>

<!-- Audio attributes -->
<audio controls autoplay loop muted preload="metadata">
    <source src="audio.mp3" type="audio/mpeg">
</audio>
```

### Iframe
```html
<!-- Embed external content -->
<iframe src="https://example.com" 
        width="800" 
        height="600" 
        title="External Content"
        loading="lazy">
</iframe>

<!-- YouTube embed -->
<iframe width="560" height="315"
        src="https://www.youtube.com/embed/VIDEO_ID"
        title="YouTube video player"
        frameborder="0"
        allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
        allowfullscreen>
</iframe>
```

### SVG
```html
<!-- Inline SVG -->
<svg width="100" height="100" xmlns="http://www.w3.org/2000/svg">
    <circle cx="50" cy="50" r="40" fill="blue" />
</svg>

<!-- SVG as image -->
<img src="image.svg" alt="SVG image">
```

---

## Meta Tags & SEO

### Essential Meta Tags
```html
<head>
    <!-- Character encoding -->
    <meta charset="UTF-8">
    
    <!-- Viewport -->
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    
    <!-- Description -->
    <meta name="description" content="Page description (150-160 characters)">
    
    <!-- Keywords -->
    <meta name="keywords" content="keyword1, keyword2, keyword3">
    
    <!-- Author -->
    <meta name="author" content="Author Name">
    
    <!-- Robots -->
    <meta name="robots" content="index, follow">
    
    <!-- Canonical URL -->
    <link rel="canonical" href="https://example.com/page">
    
    <!-- Language -->
    <html lang="en">
</head>
```

### Open Graph (Social Media)
```html
<!-- Open Graph Meta Tags -->
<meta property="og:title" content="Page Title">
<meta property="og:description" content="Page description">
<meta property="og:image" content="https://example.com/image.jpg">
<meta property="og:url" content="https://example.com/page">
<meta property="og:type" content="website">
<meta property="og:site_name" content="Site Name">

<!-- Twitter Card -->
<meta name="twitter:card" content="summary_large_image">
<meta name="twitter:site" content="@username">
<meta name="twitter:title" content="Page Title">
<meta name="twitter:description" content="Page description">
<meta name="twitter:image" content="https://example.com/image.jpg">
```

### Other Important Tags
```html
<!-- Theme color (mobile browsers) -->
<meta name="theme-color" content="#4285f4">

<!-- Apple touch icon -->
<link rel="apple-touch-icon" href="icon.png">

<!-- Favicon -->
<link rel="icon" type="image/x-icon" href="favicon.ico">
<link rel="icon" type="image/png" sizes="32x32" href="favicon-32x32.png">

<!-- Web app manifest -->
<link rel="manifest" href="/manifest.json">

<!-- Refresh/Redirect -->
<meta http-equiv="refresh" content="30">
<meta http-equiv="refresh" content="0; url=https://example.com">
```

---

## Accessibility

### ARIA (Accessible Rich Internet Applications)
```html
<!-- Landmarks -->
<nav role="navigation" aria-label="Main navigation">
    <!-- navigation links -->
</nav>

<main role="main">
    <!-- main content -->
</main>

<!-- ARIA attributes -->
<button aria-label="Close dialog" aria-pressed="false">
    <span aria-hidden="true">&times;</span>
</button>

<!-- ARIA live regions -->
<div role="alert" aria-live="polite" aria-atomic="true">
    Status message
</div>

<!-- ARIA expanded (accordion) -->
<button aria-expanded="false" aria-controls="content-1">
    Toggle Content
</button>
<div id="content-1" hidden>
    Content here
</div>

<!-- ARIA describedby -->
<input type="text" id="username" aria-describedby="username-help">
<span id="username-help">Username must be 3-20 characters</span>

<!-- ARIA labelled by -->
<div role="dialog" aria-labelledby="dialog-title">
    <h2 id="dialog-title">Dialog Title</h2>
</div>

<!-- Screen reader only text -->
<span class="sr-only">Additional context for screen readers</span>
```

### Accessibility Best Practices
```html
<!-- Always use alt text for images -->
<img src="logo.jpg" alt="Company Logo">

<!-- Decorative images -->
<img src="decoration.jpg" alt="" role="presentation">

<!-- Use semantic HTML -->
<button>Click me</button>  <!-- Good -->
<div onclick="...">Click me</div>  <!-- Bad -->

<!-- Form labels -->
<label for="email">Email:</label>
<input type="email" id="email" name="email">

<!-- Skip navigation link -->
<a href="#main-content" class="skip-link">Skip to main content</a>
<main id="main-content">
    <!-- content -->
</main>

<!-- Language of parts -->
<p>This is in English, but <span lang="es">esto es en español</span>.</p>

<!-- Tab index -->
<div tabindex="0">Focusable element</div>
<div tabindex="-1">Programmatically focusable</div>

<!-- Focus management -->
<button autofocus>Primary Action</button>
```

---

## Advanced Concepts

### HTML5 APIs & Attributes

#### Data Attributes
```html
<div data-user-id="12345" data-role="admin" data-status="active">
    User Info
</div>

<script>
    const element = document.querySelector('div');
    console.log(element.dataset.userId); // "12345"
    console.log(element.dataset.role);   // "admin"
</script>
```

#### Content Editable
```html
<div contenteditable="true">
    This content is editable
</div>

<p contenteditable="false">This cannot be edited</p>
```

#### Draggable
```html
<div draggable="true" ondragstart="drag(event)">
    Drag me
</div>

<div ondrop="drop(event)" ondragover="allowDrop(event)">
    Drop zone
</div>
```

#### Download Attribute
```html
<a href="image.jpg" download="custom-filename.jpg">
    Download Image
</a>
```

#### Spellcheck
```html
<textarea spellcheck="true"></textarea>
<input type="text" spellcheck="false">
```

#### Translate
```html
<p translate="no">Brand Name</p>
<p translate="yes">Translatable content</p>
```

### Template & Slot
```html
<!-- Template (not rendered) -->
<template id="card-template">
    <div class="card">
        <h3></h3>
        <p></p>
    </div>
</template>

<script>
    const template = document.getElementById('card-template');
    const clone = template.content.cloneNode(true);
    document.body.appendChild(clone);
</script>
```

### Web Components
```html
<!-- Custom element definition -->
<my-component></my-component>

<!-- Shadow DOM example -->
<template id="my-template">
    <style>
        /* Scoped styles */
        p { color: blue; }
    </style>
    <p><slot name="content"></slot></p>
</template>

<my-component>
    <span slot="content">Slotted content</span>
</my-component>
```

### Performance Optimization

#### Lazy Loading
```html
<!-- Lazy load images -->
<img src="image.jpg" loading="lazy" alt="Description">

<!-- Lazy load iframes -->
<iframe src="content.html" loading="lazy"></iframe>
```

#### Preloading Resources
```html
<!-- Preload -->
<link rel="preload" href="font.woff2" as="font" type="font/woff2" crossorigin>
<link rel="preload" href="critical.css" as="style">
<link rel="preload" href="hero-image.jpg" as="image">

<!-- Prefetch -->
<link rel="prefetch" href="next-page.html">

<!-- Preconnect -->
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="dns-prefetch" href="https://api.example.com">

<!-- Module preload -->
<link rel="modulepreload" href="module.js">
```

#### Async & Defer Scripts
```html
<!-- Async: Download parallel, execute ASAP -->
<script src="analytics.js" async></script>

<!-- Defer: Download parallel, execute after HTML parsed -->
<script src="app.js" defer></script>

<!-- Module -->
<script type="module" src="module.js"></script>
```

### HTML Entity References
```html
<!-- Common entities -->
&lt;        <!-- < -->
&gt;        <!-- > -->
&amp;       <!-- & -->
&quot;      <!-- " -->
&apos;      <!-- ' -->
&nbsp;      <!-- Non-breaking space -->
&copy;      <!-- © -->
&reg;       <!-- ® -->
&trade;     <!-- ™ -->
&euro;      <!-- € -->
&pound;     <!-- £-->
&yen;       <!-- ¥ -->
&#8594;     <!-- → -->
&#8592;     <!-- ← -->
```

### Microdata (Schema.org)
```html
<div itemscope itemtype="https://schema.org/Person">
    <span itemprop="name">John Doe</span>
    <img src="john.jpg" itemprop="image" alt="John Doe">
    <span itemprop="jobTitle">Software Engineer</span>
    <div itemprop="address" itemscope itemtype="https://schema.org/PostalAddress">
        <span itemprop="streetAddress">123 Main St</span>
        <span itemprop="addressLocality">City</span>
    </div>
    <a href="mailto:john@example.com" itemprop="email">john@example.com</a>
</div>
```

### Best Practices

1. **Always use semantic HTML** - Use proper elements for their intended purpose
2. **Include alt text** - Make content accessible to screen readers
3. **Use proper heading hierarchy** - Don't skip heading levels (h1 → h2 → h3)
4. **Validate your HTML** - Use W3C validator to catch errors
5. **Keep it clean** - Separate content (HTML) from presentation (CSS) and behavior (JS)
6. **Mobile-first** - Always include viewport meta tag
7. **Performance** - Lazy load images and use appropriate preload hints
8. **Accessibility** - Use ARIA attributes when needed, but prefer semantic HTML
9. **SEO** - Include proper meta tags and structured data
10. **Progressive enhancement** - Content should work without JavaScript

---

## Quick Reference

### Global Attributes (work on any element)
```html
id="unique-id"
class="class-name another-class"
style="color: red; font-size: 16px;"
title="Tooltip text"
lang="en"
dir="ltr"  <!-- or "rtl" for right-to-left -->
tabindex="0"
hidden
contenteditable="true"
draggable="true"
spellcheck="true"
translate="yes"
data-*="custom data"
```

### Block-level vs Inline Elements

**Block-level:** `<div>`, `<p>`, `<h1>-<h6>`, `<ul>`, `<ol>`, `<li>`, `<section>`, `<article>`, `<header>`, `<footer>`, `<nav>`, `<aside>`, `<main>`, `<form>`, `<table>`, `<blockquote>`, `<pre>`, `<canvas>`

**Inline:** `<span>`, `<a>`, `<strong>`, `<em>`, `<img>`, `<input>`, `<button>`, `<label>`, `<select>`, `<textarea>`, `<code>`, `<abbr>`, `<cite>`, `<small>`, `<sub>`, `<sup>`

---

**Pro Tip:** HTML is forgiving, but writing clean, semantic, valid HTML makes your code more maintainable, accessible, and SEO-friendly!