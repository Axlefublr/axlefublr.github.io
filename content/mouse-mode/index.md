+++
title = 'the mouse is an accessory'
date = '2026-06-19'
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
I move every 5ms; initially, by 1 pixel at a time. \
Over the course of 200ms, that amount of pixels rises, maxing out at 10 pixels. \
Now that the 200ms have passed, I'm moving at the maximum speed of 10 pixels per each 5ms.

Pretty sure the speed changes linearly, so we can graph it out. Let's set the minimum to 0 instead though, so that the graph is easier to look at.
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

Setting the minimum to *0* sounds silly, but it can help the taps be more precise, as effectively there's more time for my mere human fingers to be holding the keyboard key, and it not mattering. \
I tried it for a bit, but realized that it **does** still feel kind of sluggish, so I went back to 1px. \
Worth a try! Maybe it will work great for you.

Coming back to trying to solve jaggedness.
If I try to move every **1ms** rather than 5ms, the 10 pixels maximum is waaaaaay too fast.
Even if I set that maximum to 1 pixel, it's still a bit too unwieldy. \
You'd think a minimum of 0 would be viable here, but all you get in this case is a 100 millseconds of not moving at all, and then a 100 milliseconds of moving way too fast. Not viable.

That lands me into using a repeat rate of 5ms, minimum of 1px, maximum of 9px, and time period of 210ms.
But don't just take my word for it: it's very important to play with the values to try to find what feels best for **you**.
I changed these after like *months* of using the same config, so don't be afraid to change them even if they're in your muscle memory now!
Hell, I'm even changing these values as I'm writing this blog post! \
Changed 6ms → 5ms, 1px → 0px → 1px, 200ms → 160ms → 210ms, 10px → 9px.

{{hr(id="simple-math")}}

When playing around with the values, I found a convenient (although a bit naive) way to compare the overall maximum speeds of the different settings.

100/rate\*max_step

`rate` is how often you make a nudge, `max_step` is your maximum nudge bigness, in pixels. \
My current config results in a value of 180 — I (very roughly) move 180 pixels per 100 milliseconds. \
With this simple formula you can kinda compare how fast or slow your config is, so that you don't have to fight your biases as much.

Of course it completely ignores a very important value: the period in which you speed *increases*.
But my goal with this was more so testing “at my peak, how fast am I?”

### slow sublayer

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

# buttons

The actual mouse *buttons* need to be extremely accessible.
I want it to be viable to click and *drag* all three possible buttons, while moving around at the same time.
That is why <kbd>j</kbd> is left click, <kbd>k</kbd> is right click, and <kbd>m</kbd> is middle click.

So one side, the left, is for movement. And the right side is for clickment.
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

The modifier starts to be held **immediately**, rather than only once I start to hold down the mouse button.
In some places, just *hovering* your mouse over something while holding a modifier grants you access to some different behavior.
For example on discord, you start to see more possible actions when you hover over a message with <kbd>Shift</kbd> held.

Modifier support is somewhat of a recent development of mine, and it's been a massive world opener. \
Especially in image editing programs like krita, pressing all sorts of modifiers and dragging some mouse button at the same time is *incredibly* common and powerful — now I finally have full access to that!

This is also the first quite complex thing *implementation*-wise.
I'll share my total config at the end of the blog post, but I also want to *explain* how it works.

## holding modifiers

Tapping some key starts holding down a modifier.
*Releasing* some other key releases the modifier.

We can use the kanata feature “virtual keys” for this idea.
They are somewhat similar to aliases, but unlike those, they can be sent individual press/release events, rather than being bound directly to the press and release of a physical key.
Hence *virtual* key :D

First we define virtual keys for each modifier.
```kanata
(defvirtualkeys
	shift lsft
	ctrl  lctl
	alt   lalt
	meta  lmet
)
```

Then in the mouse mode layer, we make hotkeys that send only the press event to their relevant virtual key — meaning, a press (but not release!) of a specific modifier.
```kanata
a (on-press press-vkey shift)
q (on-press press-vkey meta)
t (on-press press-vkey alt)
g (on-press press-vkey ctrl)
```

Awesome! Now when I tap <kbd>a</kbd> for example, <kbd>Shift</kbd> starts to get held. \
When I release the next mouse button, I want that <kbd>Shift</kbd> to be released.
Well, *all* currently held modifiers actually! \
And since sending a release event to a key that's not currently pressed doesn't do any strange behavior, we might as well make the <kbd>j</kbd>, <kbd>k</kbd>, <kbd>m</kbd> hotkeys send releases to **all** the modifiers!

Let's first make another virtual key for convenience:
```kanata
(defvirtualkeys
	any-modifier (multi (on-press release-vkey shift) (on-press release-vkey ctrl) (on-press release-vkey alt) (on-press release-vkey meta))
)
```

And send a tap to it, on release, in the <kbd>j</kbd>, <kbd>k</kbd>, <kbd>m</kbd> hotkeys (after sending their relevant mouse button).
```kanata
m (multi mmid (on-release tap-vkey any-modifier))
j (multi mlft (on-release tap-vkey any-modifier))
k (multi mrgt (on-release tap-vkey any-modifier))
```

You might reasonably ask: “why is `any-modifier` even a virtual key anyway?”, since it just does a series of actions and doesn't need to care about the separate press/release events.
That's because of a restriction of `on-release` and `on-press` — they *have to* “call” a *virtual* key.
They cannot call an alias, and you can't just blammo the action in there directly either.
And since I want modifiers to only be released on a *release* of a mouse button, I must make `any-modifier` a virtual key.

Multi is an interesting action: unlike `macro`, all the actions you specify inside of it are done “simultaneously, in unspecified order”.
Which is how it can handle the press/release events towards `mlft` while *also* handling the release event of the `on-release`.

I don't go into virtual keys much there, but [erotic meta feet](@/kanata-layers/index.md) is a blog post of mine that shows the power and importance of them, when it comes to making composite layers. Have a read if you're interested!

# scrolling

Surprisingly, the most common usecase of mouse mode for me has become scrolling.
<kbd>i</kbd> scrolls down, <kbd>o</kbd> scrolls up, <kbd>w</kbd> scrolls left, <kbd>r</kbd> scrolls right.
```
o (mwheel-up    30 60)
i (mwheel-down  30 60)
w (mwheel-left  20 60)
r (mwheel-right 20 60)
```

Normally, a single “nudge” of the scrollwheel is 120 (units??) of distance.
For my [smooth scrolling improvements](@/smooth-scrolling/index.md) in firefox, I changed the distance from 120 to 60, but it's a *tradeoff* rather than direct improvement: some applications get confused and act weirdly due to my scroll nudge distance not being divisible by 120.
But god, the scrolling in the browser I get thanks to this is just *unbeatable*.

Initially I had vertical scrolling on <kbd>,</kbd> and <kbd>.</kbd>, but I realized that my hand naturally rests *forwards*, where keeping fingers on <kbd>i</kbd> and <kbd>o</kbd> is actually a lot more comfortable! \
So now it is *them* that I use for scrolling while reading blog posts, manga, documentation, etc.

To accompany the slow-ish wheel scrolling, I also add <kbd>PageDown</kbd> on <kbd>.</kbd> and <kbd>PageUp</kbd> on <kbd>,</kbd>. \
Whenever I need to scroll for a large ass chunk and realize holding <kbd>i</kbd> is a bit too long, I start holding <kbd>.</kbd> instead and I get where I wanted to get to very quickly 😌

# holding frame

For normal mouse usage, holding down space and executing whatever click or drag I want to do works well.
But since my mouse mode *scrolling* is so good, I often find myself using it over all other possible options.

I was reading some manga and found myself permanently holding <kbd>Space</kbd>, essentially; which is quite unergonomic!

This is why I made myself a way to *toggle into* mouse mode.
While holding <kbd>Space</kbd>, I can tap <kbd>n</kbd>, and then release <kbd>Space</kbd>.
Now mouse mode is active even without me holding anything!
Thanks to this, I can *just* press the scrolling hotkeys to read manga comfortably 😌
Although after making this, I started toggling into mouse mode most times overall 🤷‍♀️

Making it work *well* was pretty damn complicated, though.

In terms of the kanata configuration, I can enter mouse mode in two different ways, kinda.
The first is me holding <kbd>Space</kbd> from the base layer.
The second is me tapping <kbd>n</kbd> while holding <kbd>Space</kbd>.

The two enterers need to have slightly different behavior because of two things.
1. I want modifiers to be released not just on the next mouse key release, but also if I exit mouse mode altogether.
2. While I'm *toggled into* mouse mode, I want a helpful icon to be displayed in my waybar. But I *don't* want it to appear if I'm *just* holding <kbd>Space</kbd>

The first requirement comes with a particularly funny complication.
Imagine I started holding <kbd>Space</kbd>, and tapped <kbd>a</kbd> to give myself <kbd>Shift</kbd> for the next click.
Then I realize that the mouse usage got hands, and decide to toggle into mouse mode for ergonomics; so I tap <kbd>n</kbd>.
I have now *stopped holding <kbd>Space</kbd>*.
Even though I'm still in the mouse layer because I just toggled into it, I *did* release <kbd>Space</kbd> — which is one of the triggers for releasing modifiers.

So instead of giving myself ergonomics, I accidentally clear modifiers when I go from mouse mode to... mouse mode. lol \
Here's how I handled every concern!
```kanata
spc (tap-hold-release $tt $ht spc (multi (layer-toggle space) (on↑ tap-vkey space)))
```
In the base layer, holding the <kbd>Space</kbd> key holds the `space` layer (mouse mode). Once I release the <kbd>Space</kbd> key, a virtual key (also named `space`) is tapped.
The semantic here is “properly handle what should happen when <kbd>Space</kbd> is released”
```
modifier (multi (on↓ release-vkey shift) (on↓ release-vkey ctrl) (on↓ release-vkey alt) (on↓ release-vkey meta))
space (switch ((not (layer space)))
	(on↓ tap-vkey modifier)
break)
```

I then define the `modifier` virtual key, that releases all modifiers, and the `space` virtual key.
When the `space` virtual key is tapped, it taps the `modifier` virtual key, but ONLY if I am currently *not* in the space layer (mouse mode).

If I release <kbd>Space</kbd> normally, I immediately exit mouse mode and therefore the modifiers are cleared.
But if I release <kbd>Space</kbd> after tapping <kbd>n</kbd> to toggle into mouse mode, then I *remain* in mouse mode even after releasing <kbd>Space</kbd>, so the `modifier` virtual key is not tapped, and my prepared modifiers are kept.

Now let's look at what the <kbd>n</kbd> hotkey in mouse mode actually does.
```
n (on↓ toggle-vkey mouse-mode)
```
When I press <kbd>n</kbd>, the `mouse-mode` virtual key is released if it is pressed, and pressed if it is released.
Toggled, basically. Kind of like capslock and such. \
In practical effect, the first time I press it, I toggle into mouse mode, and can safely release <kbd>Space</kbd>.
Then to *exit* mouse mode, I **must** press specifically <kbd>n</kbd> again, to toggle out of mouse mode, and quit it.

```
mouse-mode (multi (cmd fish -c "echo 󰇀 >~/.local/share/mine/waybar-mouse-mode") (layer-toggle space) (on↑ tap-vkey disable-mouse-mode))
disable-mouse-mode (multi (cmd fish -c "echo >~/.local/share/mine/waybar-mouse-mode") (on↓ tap-vkey space))
```

In the `mouse-mode` virtual key, we first update the waybar widget with that echo command — so that pressing <kbd>n</kbd> in mouse mode *does* reveal the widget, but the normal holding <kbd>Space</kbd> doesn't. \
Then we hold the `space` layer (just like holding the <kbd>Space</kbd> key also does), and finally, we wait for the `mouse-mode` virtual key to be released, after which we will tap `disable-mouse-mode`. \
Note that we are interacting with the `space` *layer*, the `space` virtual key that releases modifiers is not used here!

Another note: we are waiting  for the `mouse-mode` virtual key to be released, not for <kbd>n</kbd> to be released.
Pressing <kbd>n</kbd> *toggles* `mouse-mode`, so the first time I press <kbd>n</kbd> I put `mouse-mode` into pressed-down state, and the second time I press <kbd>n</kbd> I *release* `mouse-mode`.

So essentially that `on↑` is waiting for me to press <kbd>n</kbd> another time.

Once I do, `mouse-mode` is released, and with it, the `space` layer is also released — I leave mouse mode.
Then, the `disable-mouse-mode` virtual key is tapped.
It clears the waybar widget, and taps the `space` virtual key (now *key*, **not** *layer*!), which releases all held modifiers.

To be fair I did warn you that it's complicated, lol :p

## layer responsibility

`layer-toggle`, less confusingly known as `layer-while-held`, gives you a promise: while you are holding the trigger key, I will hold this layer active for you.
```kanata
spc (tap-hold-release $tt $ht spc (multi (layer-while-held space) (on↑ tap-vkey space)))
```
The <kbd>Space</kbd> hotkey will hold the `space` layer (mouse mode) as long as I hold the <kbd>Space</kbd> key.

So why, once we release <kbd>Space</kbd> after tapping <kbd>n</kbd>, don't we leave mouse mode? Shouldn't the release of <kbd>Space</kbd> trigger a layer exit?

Apparently that's not quite how kanata layering works.
How I can best describe it, is that it's a *responsibility* system.

Imagine a glass plate.
If nobody is holding it, it will drop down on the floor and shatter.
But if at least one person is holding it, it will not drop, and stays safe.

When I start holding <kbd>Space</kbd>, one person is now holding the plate.
I tap <kbd>n</kbd>, and now a second person joins in — two people are now holding it.
I release <kbd>Space</kbd>, and the first person is no longer holding the plate.
But the second person still is!

Releasing <kbd>Space</kbd> does **not** make the first person violently grab the plate out of the hands of person B to then throw it on the ground; they just pull their hands away carefully, and are no longer responsible for the plate — *person B is*.
```
mouse-mode (multi (…) (layer-while-held space) (…))
```

The `mouse-mode` virtual key, which when pressed / held down, *also* holds down the `space` layer, is person B in this analogy.
That's how releasing <kbd>Space</kbd> *doesn't* instantly leave mouse mode, if you've tapped <kbd>n</kbd>.

# zooming hotkeys

There is an ongoing [niri pr](https://github.com/niri-wm/niri/pull/3246) that adds zoom functionality.
I merge it into my [personal niri fork](https://github.com/Axlefublr/niri), and add hotkeys for zooming into my mouse mode.
```
u (cmd niri msg action set-zoom-level +1)
p (tap-hold-release $tt $ht (cmd niri msg action set-zoom-level -- -1) (cmd niri msg action set-zoom-level 1))
```

Tapping <kbd>u</kbd> simply increments the zoom level. Increments *exponentially*, with my niri zoom config:
```kdl
zoom {
    movement-mode "centered"
    increment-type "exponential"
    max-zoom 10000.0
}
```

Behavior of hotkey <kbd>p</kbd> is a bit more fancy though.
Tapping it decrements the zoom level, but *holding* it **resets** the zoom level.

My insanely large `max-zoom` is intentional; it's a bit of a workaround.
When the current zoom level is not a whole number (integer), it looks smoothed out and really ugly.
Let's imagine, for simplicity of the explanation, that every zoom increment multiplies the current zoom value by 2, and every zoom decrement divides the current zoom value by 2.

By default, I think the maximum zoom level caps out at 10. \
From no zoom (1), I start increasing: 1 → 2 → 4 → 8 → 16 \
16 is higher than 10, so the zoom is capped at 10.

Cool, now let's zoom back *out*. \
10 → 5 → 2.5 \
Two point *five*. Not a whole number. \
Now my screen looks cummed on, smoothed out rather than sharp, and super ugly.

But if I set a really high max zoom level, I never experience this truncation: \
1 → 2 → 4 → 8 → 16 → 32 → 64 → 128 → 64 → 32 → … → 1

*However*. I still do due to another reason. Zoom animations.

During the animation, the current zoom level is gradually changed.
If you press the zoom increment hotkey while an animation is ongoing, you will be multiplying the *current* zoom level, rather than the zoom level that you're in the animation to get to.

8 → 16, you press the hotkey somewhere in the middle, at for example 10.125, and you multiply *that* now, getting to 20.25.
Not a whole number, screen is cum again.

I fix this in my fork, by rounding the target zoom level in exponential increments.
So instead of 20.25, I arrive at just 20. And in the earlier example, I arrive at 3 instead of 2.5.

{{video(path="zooming")}}

I very often use zoom for my various cssing adventures; being able to look that closely at pixels is incredibly helpful.
Plus I wear glasses, and even despite them, computer things are often too small for me to comfortably look at.
Most websites, for instance, I zoom to sometimes 150% 🤯

# niche ergonomics

Toggling in and out of mouse mode is totally normal and intentional, so I don't usually try to make my various other hotkeys available from mouse mode.
But one particular workflow started to get on my nerves with how often I needed to toggle mouse mode.

Well, I say workflow, but it's actually one of my forms of rest.

Looking for wallpapers. I have a whole sidebery workspace where all I do is look at art on pixiv, with the intent to find new wallpapers.
You can find the wallpapers that are currently in my rotation [here](https://github.com/Axlefublr/wallpapers).

There's quite a lot of different things I need to do during this.
Have a look first, I'll explain after.

{{video(path="wallpaper")}}

I found a wallpaper I like enough to send to my [discord server](https://discord.gg/bgVSg362dK)'s “wallpapers” channel.
I open it in full so that it's not preview quality, right click, copy image, compress it{{fn(i=2)}}, disable mouse mode to go to the first workspace and paste it in. \
I jump back to workspace 2, enable mouse mode again, and continue looking at wallpapers.
I use a hotkey to jump into the grey area between the recommended images, so that no annoying ui pops up as I scroll.
As I find wallpapers that look interesting, I middle click on them and jump back to the center with the grey area hotkey.
Once it looks like I exhausted interesting-looking wallpapers, I close the current tab, and repeat the process.

Not every wallpaper looks good enough for me to want to send it in the “wallpapers” channel, so toggling in and out of mouse mode for that isn't too bad.
But everything else I *do* want to do while staying in mouse mode.
```
h (cmd ydotool mousemove -a 386 300)
c y
v (tap-hold-release $tt $ht C-w XX)
x (cmd fish -c r#"show_clipboard_image -F -e "swayimg.viewer.set_default_scale('fill')""#)
```

While in mouse mode, <kbd>v</kbd> sends <kbd>ctrl+w</kbd>, the hotkey to close the current tab.
I make it a tapholdie that does nothing on holdie because I'm *so* used to that timing, that it's been many times that I accidentally closed multiple tabs at once. \
<kbd>h</kbd> uses `ydotool` to move my mouse to a predetermined spot, as navigating back to it is just *that* common of a usecase. \
As you may know, I [hate pressing y](@/stray-from-defaults/index.md), so I make <kbd>c</kbd> send <kbd>y</kbd>, so that I can “Cop**y** image” after right clicking on it.

<kbd>x</kbd> I forgot to showcase in the recording: it displays the image in my clipboard with my image viewer.
Say I like the image I'm looking at enough to consider adding it to my collection; I'll open it in full, copy it to my clipboard, and then press <kbd>x</kbd> to see how it would look like as an actual wallpaper.

{{video(path="view-with-x")}}

# convenience layers and their inconveniences

The wallpaper restflow is only one of the things that make sense to add into mouse mode.
There are quite a few other hotkeys that make a lot more sense to be in mouse mode than in not.
1. The image compressing hotkey I mentioned
2. Take selected text, and update current tab's url to point to it (by using a text fragment)
3. Annotate the screen using satty
4. Display current mouse cursor position in a notification
5. Use firefox's screenshotting functionality (lets you target html elements, which can let you screenshot a larger space than fits on your screen)
6. Ublock *zap* element (to remove temporarily)
7. Ublock pick element (to remove permanently)
8. Inspect element
9. Toggle developer tools
10. Copy hex color under the cursor

I was already pretty much out of available hotkeys even before all these, but 10 actions really do require an extra mode. Or two. Yeah, two. \
The first I access by holding <kbd>'</kbd> while in mouse mode, and the other by holding <kbd>Capslock</kbd> while in mouse mode.

Read [erotic meta feet](@/kanata-layers/index.md) to understand the below explanation better. \
“Read WHAT??!” — you heard me 😌. Read erotic meta feet.

Unlike usual composite layers, we have a bit of a fun special case.
Normally you get into a composite layer by physically holding two (or more) keys.
But in this case, I *may* be holding <kbd>Space</kbd>, **or** I am “virtually” holding <kbd>Space</kbd> because I've already tapped <kbd>n</kbd>.

So instead of releasing all the space layers on releasing <kbd>Space</kbd>, we release all composite layers on *leaving mouse mode*.

```
(deflayermap (space-quote)
	c (cmd fish -c clipboard_image_save)
	e (cmd fish -c fragmentize_url)
	s (multi (cmd fish -c annotate_screen) (on↑ tap-vkey modifier-zoom))
	w (cmd fish -c mouse_position)
	d C-S-s
	x C-A-b
	z C-A-y
)

(deflayermap (caps-space)
	i C-S-c
	o C-S-i
	k (cmd fish -c pick_and_copy_color)
)
```
First we create the composite layers with our functionality, then edit the `space` virtual key to handle releasing them.
```
space (switch ((not (layer space)))
	(multi (layer↑ space) (layer↑ caps-space) (layer↑ space-quote) (on↓ tap-vkey modifier))
break)
```

Mouse mode, as well as all of its composite layers, will now be released when I release <kbd>Space</kbd>, but only if didn't tap <kbd>n</kbd>.
It will also release them on the second tap of <kbd>n</kbd>.

Both <kbd>Capslock</kbd> and <kbd>'</kbd> have layers of their own, so in the usual erotic meta feet way, I also need to handle releasing them all, in their respective virtual keys:
```
caps (multi (layer↑ caps) (layer↑ caps-space))
quote (multi (layer↑ quote) (layer↑ space-quote) (layer↑ quote-j) (layer↑ quote-n))
```

Now we can safely add the hotkeys to get into composite layers into mouse mode:
```
caps (tap-hold-release $tt $ht esc (multi (layer-toggle caps) (layer-toggle caps-space) (on↑ tap-vkey caps)))
' (multi (layer-toggle quote) (layer-toggle space-quote) (on↑ tap-vkey quote))
```

I make <kbd>Capslock</kbd> act as <kbd>Escape</kbd>, just like how it also does outside of mouse mode; <kbd>Escape</kbd> is common everywhere!

```
(deflayermap (quote)
	spc (multi (layer-toggle space) (layer-toggle space-quote) (on↑ tap-vkey space))
	; …
)
(deflayermap (caps)
	spc (multi (layer-toggle space) (layer-toggle caps-space) (on↑ tap-vkey space))
	; …
)
```
And finally, I don't forget to add <kbd>Space</kbd> as an enterer to my `quote` and `caps` layers. As, again, those keys have their own layers.

# flexing

And we're finally done! You can now look at the *entire* config, now that it's explained fully in this blog post;
And after it, see me actually *use* mouse mode in some screen recordings.

```
(defvirtualkeys
	; …
	caps (multi (layer↑ caps) (layer↑ caps-space))
	quote (multi (layer↑ quote) (layer↑ space-quote) (layer↑ quote-j) (layer↑ quote-n))
	shift lsft
	ctrl  lctl
	alt   lalt
	meta  lmet
	modifier (multi (on↓ release-vkey shift) (on↓ release-vkey ctrl) (on↓ release-vkey alt) (on↓ release-vkey meta))
	modifier-zoom (multi (on↓ tap-vkey modifier) (cmd niri msg action set-zoom-level 1))
	space (switch ((not (layer space)))
		(multi (layer↑ space) (layer↑ caps-space) (layer↑ space-quote) (on↓ tap-vkey modifier))
	break)

	disable-mouse-mode (multi (cmd fish -c "echo >~/.local/share/mine/waybar-mouse-mode") (on↓ tap-vkey space))
	mouse-mode (multi (cmd fish -c "echo 󰇀 >~/.local/share/mine/waybar-mouse-mode") (layer-toggle space) (on↑ tap-vkey disable-mouse-mode))
	; …
)

(defsrc)
(deflayermap (default)
	; …
	caps (tap-hold-release $tt $ht esc (multi (layer-toggle caps) (on↑ tap-vkey caps)))
	' (tap-hold-release $tt $ht ' (multi (layer-toggle quote) (on↑ tap-vkey quote)))
	spc (tap-hold-release $tt $ht spc (multi (layer-toggle space) (on↑ tap-vkey space)))
	; …
)

(deflayermap (quote)
	; …
	spc (multi (layer-toggle space) (layer-toggle space-quote) (on↑ tap-vkey space))
	; …
)

(deflayermap (caps)
	; …
	spc (multi (layer-toggle space) (layer-toggle caps-space) (on↑ tap-vkey space))
	; …
)

(deflayermap (space)
	caps (tap-hold-release $tt $ht esc (multi (layer-toggle caps) (layer-toggle caps-space) (on↑ tap-vkey caps)))
	' (multi (layer-toggle quote) (layer-toggle space-quote) (on↑ tap-vkey quote))
	spc enter
	n (on↓ toggle-vkey mouse-mode)
	l (layer-toggle mouse-slow)

	h (cmd ydotool mousemove -a 386 300)
	y XX
	z XX
	c y

	u (cmd niri msg action set-zoom-level +1)
	p (tap-hold-release $tt $ht (cmd niri msg action set-zoom-level -- -1) (cmd niri msg action set-zoom-level 1))
	v (tap-hold-release $tt $ht C-w XX)
	; (multi (cmd fish -c screenshot_select) (on↑ tap-vkey modifier))
	x (cmd fish -c r#"show_clipboard_image -F -e "swayimg.viewer.set_default_scale('fill')""#)

	a (on↓ press-vkey shift)
	q (on↓ press-vkey meta)
	t (on↓ press-vkey alt)
	g (on↓ press-vkey ctrl)

	m (multi mmid (on↑ tap-vkey modifier))
	j (multi mlft (on↑ tap-vkey modifier))
	k (multi mrgt (on↑ tap-vkey modifier))
	, pgup
	. pgdn

	o (mwheel-up    30 60)
	i (mwheel-down  30 60)
	w (mwheel-left  20 60)
	r (mwheel-right 20 60)

	s (movemouse-accel-left  5 210 1 9)
	d (movemouse-accel-down  5 210 1 9)
	e (movemouse-accel-up    5 210 1 9)
	f (movemouse-accel-right 5 210 1 9)
)

(deflayermap (mouse-slow)
	s (movemouse-left  20 1)
	d (movemouse-down  20 1)
	e (movemouse-up    20 1)
	f (movemouse-right 20 1)
)

(deflayermap (space-quote)
	c (cmd fish -c clipboard_image_save)
	e (cmd fish -c fragmentize_url)
	s (multi (cmd fish -c annotate_screen) (on↑ tap-vkey modifier-zoom))
	w (cmd fish -c mouse_position)
	d C-S-s
	x C-A-b
	z C-A-y
)

(deflayermap (caps-space)
	i C-S-c
	o C-S-i
	k (cmd fish -c pick_and_copy_color)
)
```

Well I say “in full”, but I'm obviously `; …`ing the mouse mode-irrelevant parts out of here.

{{video(path="discord-mouse-usage")}}

Despite discord having a fair bit of keyboard navigation, I find mouse mode so comfy that I often use it over it 🤷‍♀️

{{video(path="horizontal-scrolling")}}

Easy, pleasant, smooth horizontal scrolling, followed by some vertical scrolling.

{{video(path="satty")}}

I'm having performance issues while being recorded; doing something actually intentional tends to be smoother.
But yeah, I do genuinely use mouse mode for even image editing programs!

{{video(path="nubby")}}

Yes, I'm serious. I sometimes play **games** with mouse mode.

Get inspired goddammit! Everyone should have **a**, and should have an **amazing** mouse mode. No reason to be fine with compromises, when perfection already exists!

# footnotes

{{hn(i=1)}} It's actually what I went with at first! Before I realized I could actually achieve comfortable drag.

{{hn(i=2)}} Discord without nitro lets you upload only up to 10mb, so I have a hotkey that compresses the image in clipboard to fit into that restraint. In the recording you see a notification briefly flash: the compressing mechanism started and immediately stopped, because the image is under 10mb to begin with, so the original is kept.
