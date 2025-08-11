+++
title = 'mouse mode'
date = '2025-08-10'
draft = true
+++

A genuine 95% of my mouse movement doesn't involve me touching the mouse. \
It's all from the keyboard!

I mentioned mouse mode in my [blog post on home row mods](@/erm/index.md), but it's evolved since then; \
Somehow, it's even better now :3

```
(deflayermap (mouse)
	spc (layer-switch default)
	n (layer-switch mouse)
	l (layer-while-held mouse-slow)

	v C-w
	h (cmd woomer)
	/ (cmd $fsh pick_and_copy_color)
	u (cmd $fsh screenshot_select)

	caps esc
	; lmet
	x lalt
	a lsft
	c lctl

	m mmid
	j mlft
	k mrgt
	i pgup
	o pgdn

	, (mwheel-up    100 120)
	. (mwheel-down  100 120)
	w (mwheel-left  30 120)
	r (mwheel-right 30 120)

	s (movemouse-accel-left  5 300 1 7)
	d (movemouse-accel-down  5 300 1 7)
	e (movemouse-accel-up    5 300 1 7)
	f (movemouse-accel-right 5 300 1 7)
)

(deflayermap (mouse-slow)
	s (movemouse-left  15 1)
	d (movemouse-down  15 1)
	e (movemouse-up    15 1)
	f (movemouse-right 15 1)
)
```

There's an awful lot of “combinations” with the mouse, they were actually quite hard to design around. \
The most basic idea, is that you need to be able to not only *just* click the main three mouse buttons, but also *hold them* while moving the pointer.

While my arrow keys mode can gladly be on <kbd>f↓</kbd>, the mouse mode needs *both* sides of the keyboard for optimal ergonomics — so holding <kbd>Space</kbd> is what enables my mouse mode.

First, the movement of the cursor — on which keys should it be? \
As a nvim, and now helix user, I had a very natural inclination to go with hjkl. \
It was natural, for sure
