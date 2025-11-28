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

❗tapholdy has to be space for convenient usage of both sides at the same time
❗you can differentiate on press / on release events, but your action *has to* be a virtual key. think of it as if you're being required to use an alias as soon as you need to differentiate press/release. otherwise it's not more complex really
