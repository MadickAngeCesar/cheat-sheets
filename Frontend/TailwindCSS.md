# TailwindCSS Cheat Sheet - Beginner to Pro

## Table of Contents
- [Setup](#setup)
- [Layout](#layout)
- [Flexbox](#flexbox)
- [Grid](#grid)
- [Spacing](#spacing)
- [Sizing](#sizing)
- [Typography](#typography)
- [Colors](#colors)
- [Backgrounds](#backgrounds)
- [Borders](#borders)
- [Effects](#effects)
- [Filters](#filters)
- [Transitions & Animation](#transitions--animation)
- [Transforms](#transforms)
- [Interactivity](#interactivity)
- [Responsive Design](#responsive-design)
- [Dark Mode](#dark-mode)
- [Customization](#customization)
- [Plugins](#plugins)
- [Best Practices](#best-practices)

---

## Setup

### Installation
```bash
# Install via npm
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p

# With Create React App
npm install -D tailwindcss
npx tailwindcss init
```

### Configuration (tailwind.config.js)
```javascript
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
    "./src/**/*.{js,jsx,ts,tsx}",
    "./public/index.html"
  ],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

### CSS Setup
```css
/* src/index.css */
@tailwind base;
@tailwind components;
@tailwind utilities;

/* Custom styles */
@layer components {
  .btn-primary {
    @apply bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600;
  }
}
```

---

## Layout

### Display
```html
<!-- Block & Inline -->
<div class="block">Block</div>
<div class="inline-block">Inline Block</div>
<div class="inline">Inline</div>
<div class="hidden">Hidden</div>

<!-- Flex & Grid -->
<div class="flex">Flex</div>
<div class="inline-flex">Inline Flex</div>
<div class="grid">Grid</div>
<div class="inline-grid">Inline Grid</div>

<!-- Table -->
<div class="table">Table</div>
<div class="table-row">Table Row</div>
<div class="table-cell">Table Cell</div>
```

### Position
```html
<!-- Static, Relative, Absolute, Fixed, Sticky -->
<div class="static">Static</div>
<div class="relative">Relative</div>
<div class="absolute">Absolute</div>
<div class="fixed">Fixed</div>
<div class="sticky">Sticky</div>

<!-- Positioning -->
<div class="absolute top-0 left-0">Top Left</div>
<div class="absolute top-0 right-0">Top Right</div>
<div class="absolute bottom-0 left-0">Bottom Left</div>
<div class="absolute bottom-0 right-0">Bottom Right</div>

<!-- Inset -->
<div class="absolute inset-0">All sides 0</div>
<div class="absolute inset-x-0">Horizontal 0</div>
<div class="absolute inset-y-0">Vertical 0</div>

<!-- Z-index -->
<div class="z-0">z-index: 0</div>
<div class="z-10">z-index: 10</div>
<div class="z-20">z-index: 20</div>
<div class="z-50">z-index: 50</div>
<div class="z-auto">z-index: auto</div>
```

### Container
```html
<!-- Responsive container -->
<div class="container mx-auto">
  Content
</div>

<!-- Max width containers -->
<div class="max-w-sm mx-auto">Small</div>
<div class="max-w-md mx-auto">Medium</div>
<div class="max-w-lg mx-auto">Large</div>
<div class="max-w-xl mx-auto">Extra Large</div>
<div class="max-w-2xl mx-auto">2XL</div>
<div class="max-w-7xl mx-auto">7XL</div>
```

---

## Flexbox

### Flex Direction
```html
<div class="flex flex-row">Row (default)</div>
<div class="flex flex-row-reverse">Row Reverse</div>
<div class="flex flex-col">Column</div>
<div class="flex flex-col-reverse">Column Reverse</div>
```

### Flex Wrap
```html
<div class="flex flex-wrap">Wrap</div>
<div class="flex flex-wrap-reverse">Wrap Reverse</div>
<div class="flex flex-nowrap">No Wrap</div>
```

### Justify Content
```html
<div class="flex justify-start">Start</div>
<div class="flex justify-end">End</div>
<div class="flex justify-center">Center</div>
<div class="flex justify-between">Space Between</div>
<div class="flex justify-around">Space Around</div>
<div class="flex justify-evenly">Space Evenly</div>
```

### Align Items
```html
<div class="flex items-start">Start</div>
<div class="flex items-end">End</div>
<div class="flex items-center">Center</div>
<div class="flex items-baseline">Baseline</div>
<div class="flex items-stretch">Stretch</div>
```

### Align Content
```html
<div class="flex flex-wrap content-start">Start</div>
<div class="flex flex-wrap content-end">End</div>
<div class="flex flex-wrap content-center">Center</div>
<div class="flex flex-wrap content-between">Between</div>
<div class="flex flex-wrap content-around">Around</div>
```

### Flex Item Properties
```html
<!-- Flex Grow & Shrink -->
<div class="flex-1">Flex 1</div>
<div class="flex-auto">Flex Auto</div>
<div class="flex-initial">Flex Initial</div>
<div class="flex-none">Flex None</div>

<!-- Order -->
<div class="order-1">Order 1</div>
<div class="order-2">Order 2</div>
<div class="order-first">Order First</div>
<div class="order-last">Order Last</div>

<!-- Align Self -->
<div class="self-auto">Auto</div>
<div class="self-start">Start</div>
<div class="self-end">End</div>
<div class="self-center">Center</div>
<div class="self-stretch">Stretch</div>
```

### Gap
```html
<!-- Gap between flex items -->
<div class="flex gap-1">Gap 0.25rem</div>
<div class="flex gap-2">Gap 0.5rem</div>
<div class="flex gap-4">Gap 1rem</div>
<div class="flex gap-8">Gap 2rem</div>

<!-- Row & Column Gap -->
<div class="flex flex-wrap gap-x-4 gap-y-2">Custom Gap</div>
```

---

## Grid

### Grid Template Columns
```html
<!-- Number of columns -->
<div class="grid grid-cols-1">1 Column</div>
<div class="grid grid-cols-2">2 Columns</div>
<div class="grid grid-cols-3">3 Columns</div>
<div class="grid grid-cols-4">4 Columns</div>
<div class="grid grid-cols-12">12 Columns</div>

<!-- None -->
<div class="grid grid-cols-none">No Grid</div>
```

### Grid Template Rows
```html
<div class="grid grid-rows-1">1 Row</div>
<div class="grid grid-rows-2">2 Rows</div>
<div class="grid grid-rows-3">3 Rows</div>
<div class="grid grid-rows-none">No Grid</div>
```

### Grid Column Span
```html
<div class="col-span-1">Span 1</div>
<div class="col-span-2">Span 2</div>
<div class="col-span-3">Span 3</div>
<div class="col-span-full">Span Full</div>

<!-- Column Start/End -->
<div class="col-start-1">Start 1</div>
<div class="col-start-2 col-end-4">Start 2, End 4</div>
```

### Grid Row Span
```html
<div class="row-span-1">Span 1</div>
<div class="row-span-2">Span 2</div>
<div class="row-span-3">Span 3</div>
<div class="row-span-full">Span Full</div>
```

### Grid Auto Flow
```html
<div class="grid grid-flow-row">Flow Row</div>
<div class="grid grid-flow-col">Flow Column</div>
<div class="grid grid-flow-row-dense">Flow Row Dense</div>
```

### Gap
```html
<div class="grid gap-1">Gap 0.25rem</div>
<div class="grid gap-4">Gap 1rem</div>
<div class="grid gap-8">Gap 2rem</div>

<!-- Specific gaps -->
<div class="grid gap-x-4 gap-y-2">Custom Gap</div>
```

---

## Spacing

### Padding
```html
<!-- All sides -->
<div class="p-0">No padding</div>
<div class="p-1">0.25rem</div>
<div class="p-2">0.5rem</div>
<div class="p-4">1rem</div>
<div class="p-8">2rem</div>
<div class="p-16">4rem</div>

<!-- Horizontal & Vertical -->
<div class="px-4">Horizontal 1rem</div>
<div class="py-4">Vertical 1rem</div>

<!-- Individual sides -->
<div class="pt-4">Top 1rem</div>
<div class="pr-4">Right 1rem</div>
<div class="pb-4">Bottom 1rem</div>
<div class="pl-4">Left 1rem</div>
```

### Margin
```html
<!-- All sides -->
<div class="m-0">No margin</div>
<div class="m-1">0.25rem</div>
<div class="m-2">0.5rem</div>
<div class="m-4">1rem</div>
<div class="m-auto">Auto</div>

<!-- Horizontal & Vertical -->
<div class="mx-4">Horizontal 1rem</div>
<div class="my-4">Vertical 1rem</div>
<div class="mx-auto">Center horizontally</div>

<!-- Individual sides -->
<div class="mt-4">Top 1rem</div>
<div class="mr-4">Right 1rem</div>
<div class="mb-4">Bottom 1rem</div>
<div class="ml-4">Left 1rem</div>

<!-- Negative margins -->
<div class="-m-4">Negative 1rem</div>
<div class="-mt-4">Negative top 1rem</div>
```

### Space Between
```html
<!-- Space between children -->
<div class="flex space-x-4">Horizontal space 1rem</div>
<div class="flex flex-col space-y-4">Vertical space 1rem</div>

<!-- Reverse -->
<div class="flex flex-row-reverse space-x-reverse space-x-4">
  Reverse horizontal space
</div>
```

---

## Sizing

### Width
```html
<!-- Fixed widths -->
<div class="w-0">0</div>
<div class="w-1">0.25rem</div>
<div class="w-4">1rem</div>
<div class="w-64">16rem</div>

<!-- Fractional widths -->
<div class="w-1/2">50%</div>
<div class="w-1/3">33.33%</div>
<div class="w-2/3">66.67%</div>
<div class="w-1/4">25%</div>
<div class="w-3/4">75%</div>

<!-- Full & Screen -->
<div class="w-full">100%</div>
<div class="w-screen">100vw</div>

<!-- Min & Max width -->
<div class="min-w-0">Min width 0</div>
<div class="min-w-full">Min width 100%</div>
<div class="max-w-xs">Max width 20rem</div>
<div class="max-w-sm">Max width 24rem</div>
<div class="max-w-md">Max width 28rem</div>
<div class="max-w-lg">Max width 32rem</div>
<div class="max-w-full">Max width 100%</div>
```

### Height
```html
<!-- Fixed heights -->
<div class="h-0">0</div>
<div class="h-4">1rem</div>
<div class="h-64">16rem</div>

<!-- Full & Screen -->
<div class="h-full">100%</div>
<div class="h-screen">100vh</div>

<!-- Min & Max height -->
<div class="min-h-0">Min height 0</div>
<div class="min-h-full">Min height 100%</div>
<div class="min-h-screen">Min height 100vh</div>
<div class="max-h-full">Max height 100%</div>
<div class="max-h-screen">Max height 100vh</div>
```

---

## Typography

### Font Family
```html
<div class="font-sans">Sans-serif</div>
<div class="font-serif">Serif</div>
<div class="font-mono">Monospace</div>
```

### Font Size
```html
<div class="text-xs">12px</div>
<div class="text-sm">14px</div>
<div class="text-base">16px</div>
<div class="text-lg">18px</div>
<div class="text-xl">20px</div>
<div class="text-2xl">24px</div>
<div class="text-3xl">30px</div>
<div class="text-4xl">36px</div>
<div class="text-5xl">48px</div>
<div class="text-6xl">60px</div>
<div class="text-9xl">128px</div>
```

### Font Weight
```html
<div class="font-thin">100</div>
<div class="font-extralight">200</div>
<div class="font-light">300</div>
<div class="font-normal">400</div>
<div class="font-medium">500</div>
<div class="font-semibold">600</div>
<div class="font-bold">700</div>
<div class="font-extrabold">800</div>
<div class="font-black">900</div>
```

### Font Style
```html
<div class="italic">Italic</div>
<div class="not-italic">Not Italic</div>
```

### Text Alignment
```html
<div class="text-left">Left</div>
<div class="text-center">Center</div>
<div class="text-right">Right</div>
<div class="text-justify">Justify</div>
```

### Text Color
```html
<div class="text-black">Black</div>
<div class="text-white">White</div>
<div class="text-gray-500">Gray 500</div>
<div class="text-red-500">Red 500</div>
<div class="text-blue-500">Blue 500</div>
```

### Text Decoration
```html
<div class="underline">Underline</div>
<div class="line-through">Line Through</div>
<div class="no-underline">No Underline</div>
```

### Text Transform
```html
<div class="uppercase">UPPERCASE</div>
<div class="lowercase">lowercase</div>
<div class="capitalize">Capitalize</div>
<div class="normal-case">Normal Case</div>
```

### Line Height
```html
<div class="leading-none">1</div>
<div class="leading-tight">1.25</div>
<div class="leading-normal">1.5</div>
<div class="leading-relaxed">1.625</div>
<div class="leading-loose">2</div>
```

### Letter Spacing
```html
<div class="tracking-tighter">-0.05em</div>
<div class="tracking-tight">-0.025em</div>
<div class="tracking-normal">0</div>
<div class="tracking-wide">0.025em</div>
<div class="tracking-wider">0.05em</div>
<div class="tracking-widest">0.1em</div>
```

### Text Overflow
```html
<div class="truncate">Truncate with ellipsis</div>
<div class="overflow-ellipsis">Ellipsis</div>
<div class="overflow-clip">Clip</div>
```

### White Space
```html
<div class="whitespace-normal">Normal</div>
<div class="whitespace-nowrap">No Wrap</div>
<div class="whitespace-pre">Pre</div>
<div class="whitespace-pre-line">Pre Line</div>
<div class="whitespace-pre-wrap">Pre Wrap</div>
```

---

## Colors

### Text Colors
```html
<!-- Gray -->
<div class="text-gray-50">50</div>
<div class="text-gray-100">100</div>
<div class="text-gray-900">900</div>

<!-- Primary Colors -->
<div class="text-red-500">Red</div>
<div class="text-blue-500">Blue</div>
<div class="text-green-500">Green</div>
<div class="text-yellow-500">Yellow</div>
<div class="text-purple-500">Purple</div>
<div class="text-pink-500">Pink</div>
<div class="text-indigo-500">Indigo</div>
```

### Background Colors
```html
<div class="bg-white">White</div>
<div class="bg-black">Black</div>
<div class="bg-gray-500">Gray 500</div>
<div class="bg-red-500">Red 500</div>
<div class="bg-blue-500">Blue 500</div>
```

### Border Colors
```html
<div class="border border-gray-300">Gray 300</div>
<div class="border border-red-500">Red 500</div>
```

### Opacity
```html
<!-- Text opacity -->
<div class="text-blue-500 text-opacity-50">50% opacity</div>

<!-- Background opacity -->
<div class="bg-blue-500 bg-opacity-50">50% opacity</div>

<!-- Border opacity -->
<div class="border border-blue-500 border-opacity-50">50% opacity</div>
```

---

## Backgrounds

### Background Size
```html
<div class="bg-auto">Auto</div>
<div class="bg-cover">Cover</div>
<div class="bg-contain">Contain</div>
```

### Background Position
```html
<div class="bg-center">Center</div>
<div class="bg-top">Top</div>
<div class="bg-right">Right</div>
<div class="bg-bottom">Bottom</div>
<div class="bg-left">Left</div>
```

### Background Repeat
```html
<div class="bg-repeat">Repeat</div>
<div class="bg-no-repeat">No Repeat</div>
<div class="bg-repeat-x">Repeat X</div>
<div class="bg-repeat-y">Repeat Y</div>
```

### Gradients
```html
<!-- Linear gradients -->
<div class="bg-gradient-to-r from-cyan-500 to-blue-500">
  Gradient
</div>
<div class="bg-gradient-to-br from-purple-500 via-pink-500 to-red-500">
  Multi-stop Gradient
</div>

<!-- Directions -->
<div class="bg-gradient-to-t">To Top</div>
<div class="bg-gradient-to-tr">To Top Right</div>
<div class="bg-gradient-to-r">To Right</div>
<div class="bg-gradient-to-br">To Bottom Right</div>
<div class="bg-gradient-to-b">To Bottom</div>
<div class="bg-gradient-to-bl">To Bottom Left</div>
<div class="bg-gradient-to-l">To Left</div>
<div class="bg-gradient-to-tl">To Top Left</div>
```

---

## Borders

### Border Width
```html
<div class="border">1px all sides</div>
<div class="border-0">No border</div>
<div class="border-2">2px all sides</div>
<div class="border-4">4px all sides</div>
<div class="border-8">8px all sides</div>

<!-- Individual sides -->
<div class="border-t-4">Top 4px</div>
<div class="border-r-4">Right 4px</div>
<div class="border-b-4">Bottom 4px</div>
<div class="border-l-4">Left 4px</div>
```

### Border Radius
```html
<div class="rounded">0.25rem</div>
<div class="rounded-md">0.375rem</div>
<div class="rounded-lg">0.5rem</div>
<div class="rounded-xl">0.75rem</div>
<div class="rounded-2xl">1rem</div>
<div class="rounded-3xl">1.5rem</div>
<div class="rounded-full">9999px</div>

<!-- Individual corners -->
<div class="rounded-t-lg">Top corners</div>
<div class="rounded-r-lg">Right corners</div>
<div class="rounded-b-lg">Bottom corners</div>
<div class="rounded-l-lg">Left corners</div>
<div class="rounded-tl-lg">Top left</div>
<div class="rounded-tr-lg">Top right</div>
<div class="rounded-br-lg">Bottom right</div>
<div class="rounded-bl-lg">Bottom left</div>
```

### Border Style
```html
<div class="border border-solid">Solid</div>
<div class="border border-dashed">Dashed</div>
<div class="border border-dotted">Dotted</div>
<div class="border border-double">Double</div>
<div class="border-none">None</div>
```

### Divide (Between Elements)
```html
<div class="divide-y divide-gray-200">
  <div>Item 1</div>
  <div>Item 2</div>
  <div>Item 3</div>
</div>

<div class="flex divide-x divide-gray-200">
  <div>Item 1</div>
  <div>Item 2</div>
</div>
```

### Ring (Outline)
```html
<div class="ring">Ring</div>
<div class="ring-2">Ring 2px</div>
<div class="ring-4">Ring 4px</div>
<div class="ring-inset">Inset ring</div>
<div class="ring-blue-500">Blue ring</div>
<div class="ring-offset-2">Ring with offset</div>
```

---

## Effects

### Box Shadow
```html
<div class="shadow-sm">Small shadow</div>
<div class="shadow">Default shadow</div>
<div class="shadow-md">Medium shadow</div>
<div class="shadow-lg">Large shadow</div>
<div class="shadow-xl">Extra large shadow</div>
<div class="shadow-2xl">2XL shadow</div>
<div class="shadow-inner">Inner shadow</div>
<div class="shadow-none">No shadow</div>
```

### Opacity
```html
<div class="opacity-0">0%</div>
<div class="opacity-25">25%</div>
<div class="opacity-50">50%</div>
<div class="opacity-75">75%</div>
<div class="opacity-100">100%</div>
```

### Mix Blend Mode
```html
<div class="mix-blend-normal">Normal</div>
<div class="mix-blend-multiply">Multiply</div>
<div class="mix-blend-screen">Screen</div>
<div class="mix-blend-overlay">Overlay</div>
```

---

## Filters

### Blur
```html
<div class="blur-none">No blur</div>
<div class="blur-sm">Small blur</div>
<div class="blur">Default blur</div>
<div class="blur-lg">Large blur</div>
```

### Brightness
```html
<div class="brightness-50">50%</div>
<div class="brightness-100">100%</div>
<div class="brightness-150">150%</div>
```

### Contrast
```html
<div class="contrast-50">50%</div>
<div class="contrast-100">100%</div>
<div class="contrast-150">150%</div>
```

### Grayscale
```html
<div class="grayscale-0">No grayscale</div>
<div class="grayscale">100% grayscale</div>
```

### Backdrop Filter
```html
<div class="backdrop-blur-sm">Backdrop blur</div>
<div class="backdrop-brightness-50">Backdrop brightness</div>
<div class="backdrop-contrast-100">Backdrop contrast</div>
```

---

## Transitions & Animation

### Transition Property
```html
<div class="transition">All properties</div>
<div class="transition-colors">Colors only</div>
<div class="transition-opacity">Opacity only</div>
<div class="transition-transform">Transform only</div>
<div class="transition-none">No transition</div>
```

### Transition Duration
```html
<div class="duration-75">75ms</div>
<div class="duration-150">150ms</div>
<div class="duration-300">300ms</div>
<div class="duration-500">500ms</div>
<div class="duration-1000">1000ms</div>
```

### Transition Timing
```html
<div class="ease-linear">Linear</div>
<div class="ease-in">Ease In</div>
<div class="ease-out">Ease Out</div>
<div class="ease-in-out">Ease In Out</div>
```

### Transition Delay
```html
<div class="delay-75">75ms</div>
<div class="delay-150">150ms</div>
<div class="delay-300">300ms</div>
```

### Animation
```html
<div class="animate-none">No animation</div>
<div class="animate-spin">Spin</div>
<div class="animate-ping">Ping</div>
<div class="animate-pulse">Pulse</div>
<div class="animate-bounce">Bounce</div>
```

---

## Transforms

### Scale
```html
<div class="scale-0">0%</div>
<div class="scale-50">50%</div>
<div class="scale-100">100%</div>
<div class="scale-150">150%</div>

<!-- Axis-specific -->
<div class="scale-x-50">Scale X 50%</div>
<div class="scale-y-50">Scale Y 50%</div>
```

### Rotate
```html
<div class="rotate-0">0°</div>
<div class="rotate-45">45°</div>
<div class="rotate-90">90°</div>
<div class="rotate-180">180°</div>
<div class="-rotate-45">-45°</div>
```

### Translate
```html
<div class="translate-x-0">X: 0</div>
<div class="translate-x-4">X: 1rem</div>
<div class="translate-y-4">Y: 1rem</div>
<div class="-translate-x-4">X: -1rem</div>
```

### Skew
```html
<div class="skew-x-0">X: 0°</div>
<div class="skew-x-6">X: 6°</div>
<div class="skew-y-6">Y: 6°</div>
<div class="-skew-x-6">X: -6°</div>
```

### Transform Origin
```html
<div class="origin-center">Center</div>
<div class="origin-top">Top</div>
<div class="origin-top-right">Top Right</div>
<div class="origin-right">Right</div>
<div class="origin-bottom">Bottom</div>
```

---

## Interactivity

### Cursor
```html
<div class="cursor-auto">Auto</div>
<div class="cursor-pointer">Pointer</div>
<div class="cursor-not-allowed">Not Allowed</div>
<div class="cursor-wait">Wait</div>
<div class="cursor-text">Text</div>
<div class="cursor-move">Move</div>
```

### User Select
```html
<div class="select-none">No Select</div>
<div class="select-text">Text Select</div>
<div class="select-all">Select All</div>
<div class="select-auto">Auto</div>
```

### Pointer Events
```html
<div class="pointer-events-none">None</div>
<div class="pointer-events-auto">Auto</div>
```

### Resize
```html
<textarea class="resize-none">No resize</textarea>
<textarea class="resize">Resize</textarea>
<textarea class="resize-x">Resize X</textarea>
<textarea class="resize-y">Resize Y</textarea>
```

### Overflow
```html
<div class="overflow-auto">Auto</div>
<div class="overflow-hidden">Hidden</div>
<div class="overflow-visible">Visible</div>
<div class="overflow-scroll">Scroll</div>

<!-- Axis-specific -->
<div class="overflow-x-auto">X Auto</div>
<div class="overflow-y-auto">Y Auto</div>
```

---

## Responsive Design

### Breakpoints
```html
<!-- Mobile First Approach -->
<div class="w-full md:w-1/2 lg:w-1/3 xl:w-1/4">
  Responsive width
</div>

<!-- Breakpoints:
  sm: 640px   (small devices)
  md: 768px   (medium devices)
  lg: 1024px  (large devices)
  xl: 1280px  (extra large devices)
  2xl: 1536px (2x extra large devices)
-->

<!-- Hide on mobile, show on desktop -->
<div class="hidden md:block">Desktop only</div>

<!-- Show on mobile, hide on desktop -->
<div class="block md:hidden">Mobile only</div>

<!-- Responsive text -->
<h1 class="text-2xl md:text-4xl lg:text-6xl">
  Responsive Heading
</h1>

<!-- Responsive padding -->
<div class="p-4 md:p-8 lg:p-12">
  Responsive padding
</div>
```

---

## Dark Mode

### Enable Dark Mode
```javascript
// tailwind.config.js
module.exports = {
  darkMode: 'class', // or 'media'
  // ...
}
```

### Dark Mode Classes
```html
<!-- Toggle dark mode by adding 'dark' class to html/body -->
<div class="bg-white dark:bg-gray-800">
  <h1 class="text-black dark:text-white">
    Title
  </h1>
  <p class="text-gray-600 dark:text-gray-300">
    Content
  </p>
</div>

<!-- Dark mode button -->
<button class="bg-white dark:bg-gray-800 border dark:border-gray-600">
  Toggle Dark Mode
</button>
```

---

## Customization

### Extend Theme
```javascript
// tailwind.config.js
module.exports = {
  theme: {
    extend: {
      colors: {
        'brand': '#FF6B6B',
        'primary': {
          50: '#f0f9ff',
          100: '#e0f2fe',
          // ... more shades
          900: '#0c4a6e',
        }
      },
      spacing: {
        '128': '32rem',
        '144': '36rem',
      },
      borderRadius: {
        '4xl': '2rem',
      },
      fontFamily: {
        sans: ['Inter', 'sans-serif'],
        display: ['Poppins', 'sans-serif'],
      },
      fontSize: {
        '10xl': '10rem',
      }
    }
  }
}
```

### Custom Classes with @apply
```css
/* styles.css */
@layer components {
  .btn {
    @apply px-4 py-2 rounded font-semibold transition;
  }
  
  .btn-primary {
    @apply bg-blue-500 text-white hover:bg-blue-600;
  }
  
  .btn-secondary {
    @apply bg-gray-200 text-gray-800 hover:bg-gray-300;
  }
  
  .card {
    @apply bg-white rounded-lg shadow-md p-6;
  }
}
```

---

## Plugins

### Official Plugins
```bash
# Forms plugin
npm install @tailwindcss/forms

# Typography plugin
npm install @tailwindcss/typography

# Aspect ratio plugin
npm install @tailwindcss/aspect-ratio

# Line clamp plugin
npm install @tailwindcss/line-clamp
```

### Configure Plugins
```javascript
// tailwind.config.js
module.exports = {
  plugins: [
    require('@tailwindcss/forms'),
    require('@tailwindcss/typography'),
    require('@tailwindcss/aspect-ratio'),
  ],
}
```

### Using Plugins
```html
<!-- Typography plugin -->
<article class="prose lg:prose-xl">
  <h1>Article Title</h1>
  <p>Content with automatic styling...</p>
</article>

<!-- Forms plugin (automatic styling) -->
<input type="text" class="form-input">
<select class="form-select">...</select>

<!-- Aspect ratio plugin -->
<div class="aspect-w-16 aspect-h-9">
  <iframe src="..."></iframe>
</div>
```

---

## Best Practices

### Component Patterns
```html
<!-- Button Component -->
<button class="px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600 
               focus:outline-none focus:ring-2 focus:ring-blue-500 
               focus:ring-offset-2 transition">
  Click Me
</button>

<!-- Card Component -->
<div class="bg-white rounded-lg shadow-md overflow-hidden">
  <img src="..." class="w-full h-48 object-cover" alt="...">
  <div class="p-6">
    <h2 class="text-2xl font-bold mb-2">Title</h2>
    <p class="text-gray-600 mb-4">Description</p>
    <button class="text-blue-500 hover:text-blue-600">Read More →</button>
  </div>
</div>

<!-- Input Field -->
<div class="mb-4">
  <label class="block text-gray-700 text-sm font-bold mb-2" for="email">
    Email
  </label>
  <input
    class="shadow appearance-none border rounded w-full py-2 px-3 
           text-gray-700 leading-tight focus:outline-none focus:ring-2 
           focus:ring-blue-500"
    id="email"
    type="email"
    placeholder="Email"
  >
</div>
```

### Organization Tips
```html
<!-- Group related classes together -->
<!-- Layout → Spacing → Typography → Colors → Effects -->
<div class="flex items-center justify-between 
            p-4 space-x-4
            text-lg font-semibold
            bg-white text-gray-800
            shadow-md rounded-lg">
  Content
</div>

<!-- Extract repeating patterns to components -->
<!-- Use @apply in CSS or React/Vue components -->
```

### Performance
```javascript
// Purge unused CSS in production
// tailwind.config.js
module.exports = {
  content: [
    './src/**/*.{js,jsx,ts,tsx}',
    './public/index.html',
  ],
  // This removes unused styles automatically
}
```

### Maintainability
```html
<!-- Use semantic class names for reusable patterns -->
<button class="btn btn-primary">Click</button>

<!-- Avoid extremely long class lists -->
<!-- Extract to components instead -->

<!-- Use configuration for project-wide consistency -->
<!-- Define colors, spacing, etc. in tailwind.config.js -->
```

---

**Pro Tip:** Master responsive modifiers, leverage the configuration file for consistency, use @apply for repeating patterns, and always enable PurgeCSS in production!