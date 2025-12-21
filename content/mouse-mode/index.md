+++
title = 'the mouse is an accessory'
date = '2025-11-26'
draft = true
+++

Over the years, I've tried a bunch of different approaches to emulate the mouse from the keyboard. \
A lot of them end up falling into this unfortunate category, where they end up more so a gimmick than something that's comfortable to use over the real mouse. \
Finally, this is **not** the case with my latest mouse mode.

You could literally steal my mouse right now and pose me no inconvenience. \
Because the *vast* majority of my mouse usage now doesn't involve touching the mouse :D

I've [mentioned](@/erm/index.md) my mouse mode before, but it's grown a lot since then; \
In this blog post, I'll try to comprehensively explain all of the design decisions that made it amazing.

# movement

I make space a tapholdy: when briefly tapped, it acts as space; but when *held*, I enter *mouse mode*. \
In this mode, a bunch of letter keys now do various mouse movements and actions.

First, directional movement. \
You'd think I'd use <kbd>h</kbd>, <kbd>j</kbd>, <kbd>k</kbd>, <kbd>l</kbd>, right?

That's what I went for at first, but despite it feeling REALLY natural, it was at the end of the day *limited*. \
I can't comfortably do *all* of the diagonal movements! And being able to actually matters a lot in “not a gimmick”ness.

I now use <kbd>e</kbd>, <kbd>s</kbd>, <kbd>d</kbd>, <kbd>f</kbd> instead. \
Note, not <kbd>w</kbd>, <kbd>a</kbd>, <kbd>s</kbd>, <kbd>d</kbd>! (which is the next natural placement after <kbd>h</kbd>, <kbd>j</kbd>, <kbd>k</kbd>, <kbd>l</kbd>)

I touch type, so my natural resting keys are: <kbd>a</kbd>, <kbd>s</kbd>, <kbd>d</kbd>, <kbd>f</kbd>, <kbd>j</kbd>, <kbd>k</kbd>, <kbd>l</kbd>, <kbd>;</kbd>. \
It's a lot more comfortable to just move the middle finger from <kbd>d</kbd> to <kbd>e</kbd> to start moving the mouse, than to move the ring finger from <kbd>s</kbd> to <kbd>w</kbd>. \
And who does that anyway! We know that all of us are used to using <kbd>w</kbd>, <kbd>a</kbd>, <kbd>s</kbd>, <kbd>d</kbd> the “gaming” way, and *that* hand placement requires even more movement from the neutral position.

It was surprising to me that the most basic approach to a mouse mode (relative directional movement) turned out to be the bee's knees. \
A lot of my previous attempts worked by splitting the screen into sections in some way, letting me *instantly* jump to some spot. \
But something that I've now realized, is that a lot of (maybe even most) mouse movements aren't about jumping to some absolute spot, but rather moving relatively from the current position to another position that's close to it.

Context menus are the brightest example of this. \
You right click, then move relatively a bit to the ↘, click on another submenu-opener, and go maybe a bit to the ↗ and then click. \
Relative movements stringed together, basically.

If you do this specific action frequently, you can actually start to muscle memory it to an extent, if you use relative movement. \
But you can't do that with absolute as well — there's no guarantee you'll be opening the context menu *from* the same spot. \
So, position jumping approaches tend to suffer when it comes to consecutive close-by movements.

Which I think is what makes them hard to use enjoyably *as my main mouse*: pretty great for one-off clicks, but not really viable for continued mouse usage.

## speed

A common issue with *relative* movement mouse modes, is them being too fast / too slow. \
If you make yourself fast, you can approach (or outscale) the speed of section based mouse modes, but you lose *accuracy*. \
If you make yourself slow, you can click on very small elements, but getting to them is a slog.

My first approach was to make two different modes: one is fast, another is slow. \
The fast one is default, and while I'm holding <kbd>l</kbd> I switch to the slow mode.

Unfortunately just that isn't viable: kanata has a bug / misbehavior where it can't change the mouse moving speed midway. \
Say I start holding <kbd>f</kbd> to go right, and now need to become more accurate; I now start holding <kbd>l</kbd>. \
My speed doesn't change though! It only will once I *release* and then re-press <kbd>f</kbd> (while still holding <kbd>l</kbd>).

That makes it uncomfortable to *fully* rely on.
So the fast mode needs to be mostly fast, but *usable* for smaller movements as well.

Do you ever notice how you use the real mouse? \
To click on some spot, you first do a big inaccurate movement to “almost there”, and then you stop to do a few small movements to get to the exact spot you want to click. \
Turns out this is so engrained in me, that replicating it in my mouse mode works really well.

Mouse mode's speed is not constant: it starts out somewhat slow, and increases up to its maximum within 200ms. \
Thanks to this, when I make a big movement, my speed is fantasic! Then when I need to get a bit preciser, I can tap the movement keys some amount of times to make use of the slower speed, to get my cursor where I want without *needing* to enter the slow mode.

I still do have and do use slow mode quite often, mind you; it's just that I don't need it for *most* movements.

The time window where the speed increases is so small on purpose: it should mostly be to make taps viable; otherwise I want most of my movement to **not** be a slog.

Another thing I often noticed with moving the mouse from keyboard, is that it's very *jagged*. \
You can actively see the cursor *jumping*, rather than moving, and it feels very wack!

I solved this by using a quite small time period between moves: 5ms. \
It's frequent enough that mouse movement looks *smooth* and continuous. \
Of course to make it *perfect* I wanted to set it to 1ms, but then I can't do the above “taps are slow” thing as well.

Kanata's `movemouse-accel` actions change your speed by changing the amount of pixels you move by, with your every move. \
I move every 5ms; initially, by *zero* pixels (yep) at a time. \
Over the course of 200ms, that amount of pixels rises, maxing out at 10 pixels. \
Now that the 200ms have passed, I'm moving at the maximum speed of 10 pixels per each 5ms.

Pretty sure the speed changes linearly, so we can graph it out:
|Time passed|Pixels per move|
|-----------|---------------|
|0ms  | 0px|
|20ms | 1px|
|40ms | 2px|
|60ms | 3px|
|80ms | 4px|
|100ms| 5px|
|120ms| 6px|
|140ms| 7px|
|160ms| 8px|
|180ms| 9px|
|200ms|10px|

The starting speed being *0* pixels per move sounds silly, but it *really* helps the taps be more precise, as effectively there's more time for my mere human fingers to be holding the keyboard key, and it not mattering. \
Although after trying it for a bit, I realized that it does still feel kind of sluggish, so I went back to 1px. \
But worth a try! It might work perfect for you.

And so, if I try to move every **1ms**, the 10 pixels maximum is waaaaaay too fast. \
But actually, even the 0 pixel *minimum* is sort of too fast, for my usecase of “tap is slow” — 200ms is an awfully short time. \
Which is why 5ms is so far the best tradeoff that I found that feels good to use.

Very important to play with the values here; I changed these after like *months* of using the same config. \
So don't be afraid to change them even if they're in your muscle memory now! \
Hell, I'm even changing these values as I'm writing this blog post! Changed 6ms → 5ms, 1px → 0px → 1px, 200ms → 160ms
(leading to easier to access precision without losing long distance travelling speed)

The slow moving sublayer, that I access by holding <kbd>l</kbd>, is a lot simpler. \
Its speed is constant; it always moves by a single pixel, just more *rarely*. \
Currently, it does a move every 16ms; Athough with the recent (i.e. 5 minutes ago) change, I may make it even slower.

Most of my normal movements should be, and *are* easily doable with the **normal** speed. \
For example, I can move my cursor in a continuous motion onto any given message on discord 70% of the time, without needing to make a second nudge. \
Similar case for browser tabs (which are vertical for me).

The basic idea is that I shouldn't feel like I need to bring out precise mode *constantly*. \
It's most useful for more rare-ish situations, like taking (precise) screenshots or interacting with really small buttons / scrollbars / sliders; sometimes for selecting text (although double click selecting helps to not need it there).

In situations where slow mode is warranted, I first use the normal speed to get *almost there*, then release the movement key(s), hold down <kbd>l</kbd>, and correct my position. \
After I do so, I release <kbd>l</kbd>, move to the ending location (screenshots, text telect, slider, etc), and similarly use <kbd>l</kbd> to correct my position there too (if needed). \
**Very** important to not get baited into holding <kbd>l</kbd> for the entire move, most of which doesn't need to be that precise (and by extension *slow*).

{{hr(id="simple-math")}}

When playing around with the values, I found a convenient (although a bit naive) way to compare the overall maximum speeds of the different settings.

100/rate\*max_step

`rate` is how often you make a nudge, `max_step` is your maximum nudge bigness, in pixels. \
My current config results in a value of 180 — I (very roughly) move 180 pixels per 100 milliseconds. \
With this simple formula you can kinda compare how fast or slow your config is, so that you don't have to fight your biases as much.

Of course it completely ignores a very important value: the period in which you speed *increases*.
But my goal with this was more so testing “at my peak, how fast am I?”

## buttons

The actual mouse *buttons* need to be extremely accessible.
I want it to be viable to click and *drag* all three possible buttons, while moving around at the same time. \
That is why <kbd>j</kbd> is left click, <kbd>k</kbd> is right click, and <kbd>m</kbd> is middle click.

So one side, the left, is for movement. And the right side is for clickment. \
That's why it's so important that the key that enters mouse mode is *Space* — I will be using **both** sides of my keyboard, so holding something like <kbd>z</kbd> instead{{fn(i=1)}} would just be deeply unpleasant.

But aside from making dragging viable, I also made clicking with a modifier, and even *dragging* with a modifier viable in mouse mode.
|Key|Modifier|
|-|-|
|<kbd>a</kbd>|shift|
|<kbd>g</kbd>|ctrl|
|<kbd>q</kbd>|meta|
|<kbd>r</kbd>|alt|

Tapping one of those keys “enables” (holds down) the modifier until the next mouse button (<kbd>j</kbd> / <kbd>k</kbd> / <kbd>m</kbd>) release.
For example, I can tap <kbd>a</kbd>, then press and release <kbd>j</kbd> — that will result in a shift+click.
I can tap multiple modifier keys one after the other and they will all stack for that next mouse button press.

The modifier starts to be held immediately, rather than only once I start to hold down the mouse button.
In some places, just *hovering* your mouse over something while holding a modifier grants you access to some different behavior.
For example on discord, you start to see more possible actions when you hover over a message with <kbd>Shift</kbd> held.

Modifier support is somewhat of a recent development of mine, and it's been a massive world opener. \
Especially in image editing programs like krita, pressing all sorts of modifiers and dragging some mouse button at the same time is *incredibly* common and powerful — now I finally have full access to that!

This is also the first quite complex thing *implementation*-wise.
I'll share my total config at the end of the blog post, but I also want to *explain* how it works.

# footnotes

{{hn(i=1)}} It's actually what I went with at first! Before I realized I could actually achieve comfortable drag.

❗mod keys use virtual keys
❗you can differentiate on press / on release events, but your action *has to* be a virtual key. think of it as if you're being required to use an alias as soon as you need to differentiate press/release. otherwise it's not more complex really
❗smooth scrolling blog post
