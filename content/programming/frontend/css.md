For now, this space is a collection of various CSS tips to remember. This should probably be reorganized eventually.

## Make element full screen
**Problem:** I created a div to display a background image, but there was not enough content to fill the screen. The div was truncated.

#### CSS property
```css
min-height: 100vh;
```

#### In tailwindcss
```html
<div class="min-h-screen">
```

**Problem:** I have a div that does not reach the bottom of the screen. It stops after some number of pixels

#### CSS solution
Set a parent element to use flexbox with column orientation:
```css
display: flex;
flex-direction: column;
```

then tell the div to grow:
```css
flex-grow: 1;
```

This of course assumes that the parent element has a height equal to the viewport, and the div in question is the last flex item.