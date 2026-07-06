+++
title = 'testing'
date = '2026-07-06'
draft = true
+++

```css
/* Comments */

/* Block comment */
/* TODO: nested-looking /* text */ still comment */

/* Charset / Imports / Namespace */

@charset "UTF-8";

@import url("theme.css");
@import "print.css" print;
@import url(styles.css) screen and (min-width: 800px);

@namespace svg url("http://www.w3.org/2000/svg");
@namespace html "http://www.w3.org/1999/xhtml";

/* Font Face / Font Features */

@font-face {
    font-family: "Example Font";
    src: local("Arial"), url("font.woff2") format("woff2");
    font-display: swap;
}

@font-feature-values ExampleFont {
    @styleset {
        nice: 1;
        fancy: 2;
    }

    @character-variant {
        alt-a: 1;
    }

    @annotation {
        circled: 1;
    }

    @swash {
        flourish: 1;
    }

    @stylistic {
        style1: 1;
    }

    @ornaments {
        stars: 1;
    }
}

/* Counter Style */

@counter-style stars {
    system: cyclic;
    symbols: "★";
    suffix: " ";
}

/* Media Queries */

@media only screen and (min-width: 640px) and (orientation: landscape) {
    body {
        display: grid;
    }
}

/* Supports */

@supports (display: grid) and (color: color(display-p3 1 0 0)) {
    .supports {
        display: grid;
    }
}

/* Keyframes */

@keyframes fade {
    from {
        opacity: 0;
    }

    50% {
        opacity: .5;
    }

    to {
        opacity: 1;
    }
}

/* Page */

@page :first {
    margin: 2cm;
}

/* Document (obsolete but in grammar) */

@document url("https://example.com") {
    body {
        color: red;
    }
}

/* Viewport */

@viewport {
    width: device-width;
    zoom: 1;
}

/* Selectors */

*,
html,
body,
svg|circle,
*|*,
article,
#main,
.container,
.container.large,
#main.container,
div#id.class1.class2,
input[type="text"],
a[href^="https"],
a[href$=".pdf"],
a[href*="example"],
button:active,
input:checked,
input:disabled,
div:empty,
input:enabled,
li:first-child,
p:first-of-type,
:focus,
:focus-visible,
:focus-within,
:fullscreen,
:hover,
:in-range,
:invalid,
:any-link,
:link,
:visited,
:optional,
:required,
:read-only,
:read-write,
:root,
:scope,
:target,
:valid,
:dir(rtl),
:lang(en),
:nth-child(2n + 1),
:nth-last-child(even),
:nth-of-type(3),
:not(.hidden),
:is(.foo, .bar),
:where(section > p),
:has(img),
::before,
::after,
::first-letter,
::first-line,
::selection,
::placeholder,
::marker,
::backdrop,
main > section + article ~ aside,
nav ul li a,
dialog[open] {
    --primary: rebeccapurple;
    --spacing: 1rem;
    color: var(--primary);
}

/* Properties / Values */

.demo {
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: stretch;
    gap: 1rem;
    position: absolute;
    inset: 10px;
    z-index: 100;
    width: calc(100% - 2rem);
    height: min(80vh, 800px);
    margin: 10px auto;
    padding: clamp(1rem, 2vw, 3rem);
    border: 2px solid #ff8800;
    border-radius: 12px;
    outline: 2px dashed blue;
    outline-offset: 4px;
    background: linear-gradient(to bottom right, red, rgba(0, 255, 0, .5), hsl(240 100% 50%));
    background-image: repeating-radial-gradient(circle at center, white, black), conic-gradient(from 90deg at center, red, yellow, lime);
    color: currentColor;
    font: italic small-caps bold 18px/1.5 "Fira Code", monospace;
    font-feature-settings: "liga" 1;
    font-variation-settings: "wght" 600;
    text-decoration: underline wavy red;
    text-shadow: 0 0 5px black;
    box-shadow: 0 0 10px rgb(255 0 0 / 40%), inset 0 0 2px black;
    transform: translateX(20px) translateY(10px) translateZ(5px) translate3d(1px, 2px, 3px) rotate(45deg) rotateX(20deg) rotateY(30deg) rotateZ(10deg) rotate3d(1, 1, 0, 90deg) scale(.8) scaleX(2) scaleY(.5) scaleZ(3) scale3d(1, 2, 3) skew(10deg) skewX(5deg) skewY(15deg) matrix(1, 0, 0, 1, 0, 0) matrix3d(1,0,0,0, 0,1,0,0, 0,0,1,0, 0,0,0,1) perspective(500px);
    transition: opacity 300ms ease-in-out, transform .5s cubic-bezier(.2, .8, .4, 1);
    animation: fade 2s infinite alternate;
    opacity: .75;
    cursor: grab;
    filter: blur(2px) brightness(120%) contrast(110%) grayscale(20%) hue-rotate(45deg) invert(5%) saturate(150%) sepia(10%) drop-shadow(2px 2px 3px black);
    clip-path: polygon(50% 0%, 100% 100%, 0% 100%);
    shape-outside: circle(at center);
    content: attr(data-label);
    list-style-type: upper-roman;
    unicode-range: U+0000-00FF;
    image-rendering: pixelated;
    overflow: auto;
    resize: both;
    accent-color: orange;
    scrollbar-color: red blue;
    aspect-ratio: 16 / 9;
    container-type: inline-size;
    container-name: layout;
    color-scheme: dark light;
    user-select: none;
    pointer-events: auto;
    will-change: transform !important;
    !important: initial; /* intentionally invalid */
}

/* Escaped identifiers */

.\31 23 {
    color: lime;
}

#foo\+bar {
    color: cyan;
}

.custom\ name {
    color: magenta;
}
```
