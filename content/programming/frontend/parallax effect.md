I used [parallax.js](https://github.com/pixelcog/parallax.js/) to accomplish this in a website clone project I worked on for fun.

### Installation
```
npm i --save jquery-parallax.js
```

Then add these to your `<head>`:
```html
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.2.1/jquery.min.js" defer></script>  
<script src="../node_modules/jquery-parallax.js/parallax.min.js" defer></script>
```

Next create a `div` with appropriate classes. For the case of a background image, (also using tailwindcss):
```html
<div class="parallax-window min-h-screen" data-parallax="scroll" data-image-src="../img/patio.jpeg">
```