+++
title = 'every row modifiers'
date = '2025-04-14'
+++

An AI hallucinated, and god am I thankful that it did.

{{ hr(id="start") }}

I've been trying to find reasons to finally switch to kanata: I've tried it before in the past, and while it was really cool, ultimately it didn't feel worth the effort.

**But**! If I collect enough significant advantages over [xremap](https://github.com/xremap/xremap), I'll be able to power through the magnitudional task of reading through the docs, and processing the very hard concept of how remapping works in kanata.

First, my friend [amandin](https://discord.com/users/660129612256247845) *swears by* homerow mods. I've tried them out before and they didn't work out :c \
But maybe there's something special about his approach that is only possible in kanata? (spoiler: yes but not his).

My [magazine](@/magazines.md) implementation is like, 900+ lines of yaml. Hilariously, I want to add even more! But doing this with xremap is just not viable anymore... Maybe kanata has the answer, with all its extra functionality?

I was talking with chatgpt, trying to compare xremap to kanata before I decide to embark on this journey. \
It told me that xremap operates on a higher level than kanata: kanata using evdev, and xremap being on the OS level. \
For a remapping program, it's better to be lower, so this got me convinced that I should switch to kanata!

# thank the hallucinaton!

Turns out that both xremap and kanata operate on the same level (evdev). \
Thankfully I realized this only *after* I finished my switch to kanata: thinking that I'll get a performance benefit (smaller delays) helped me stay motivated :3

Little did I know that I'll be getting myself into *larger* delays! Because I now use home row mods 🥳

# every row mods

The basic idea of home row mods, is for you to make <kbd>a</kbd>, <kbd>s</kbd>, <kbd>d</kbd>, <kbd>f</kbd>, <kbd>j</kbd>, <kbd>k</kbd>, <kbd>l</kbd>, <kbd>;</kbd> into tapholdy modifiers. \
If you just tap the key, it produces the key. If you *hold* the key, it becomes a modifer like <kbd>Shift</kbd>.

The computer does not (yet) read your mind, so it needs some semantic to figure out what is a tap and what is a hold. \
Usually tapholdies wait for a timeout, after which the key will act as its hold variation. If you *released* the key before this timeout elapsed, it will fire the tap variation.

So this is pretty interesting! \
Your muscle memory is *strongly* used to keys doing their action as soon as you *press* them, not *release* them. \
When you first try home row modifiers, they feel very weird exactly for this reason.

But the biggest reason for why people might give up on home row modifiers, is because of the *home row* part. \
*Some*, but not **all** keys acting only on release feels *very* confusing and spotty! You can reasonably get used to this behavior if all keys acted the same way, but if only *some* of them do, it's way too tricky to get comfortable with!

So I came up with a 💥ive idea: I will make *every single key* tapholdy, even if I don't yet have a usecase for the hold behavior. This is just to get consistency across *all* keys I press (except <kbd>Enter</kbd> for now, incidentally), while also allowing easier addition of extra behavior onto key holds, without having to retrain my muscle memory every time.

# an important caveat

Having to actively wait for a timeout to elapse to use *any* modifier doesn't sound like a good value proposition. \
This is why kanata provides an alternative variant of the usual `tap-hold` action: `tap-hold-release`.

If you press and release a key while holding a tapholdy, it will automatically be considered as a holdy. \
This variation is our second thing that makes tapholdies viable! I hate it when *waiting* is an input; but if I can learn an alternative way to use my keyboard to circumvent it, I'm far more excited about the whole idea!

It's fairly difficult to get used to, don't get me wrong, but I assume I'll eventually get very comfortable at making sure to release keys before releasing their respective tapholdy.

Here's another variation, that sounds even better, until you actually use it :p \
`tap-hold-press` considers a tapholdy to be a holdy if you *press* another key while holding it. \
Rather than press and *release* of the above action, *just* the press is enough.

This makes using modifiers heavenly! But typing normally? Hellish! \
You don't realize how often you're pressing another key before releasing the first one, until you try to use `tap-hold-press`. \
I encourage you to try, though! There's probably a level of typing speed at which this doesn't matter that much, but at mine (~75) it's pretty unbearable.

# the goodies

Now that we've talked about how to make tapholdies viable, let me share how I made them useful for myself!

[Here's my kanata config.](https://github.com/Axlefublr/dotfiles/blob/main/kanata/kanata.kbd)

First, I made <kbd>d</kbd>/<kbd>k</kbd> to holdy into <kbd>ctrl</kbd>, <kbd>s</kbd>/<kbd>l</kbd> -> <kbd>shift</kbd>, <kbd>f</kbd>/<kbd>j</kbd> -> <kbd>alt</kbd>, and <kbd>w</kbd>/<kbd>o</kbd> -> <kbd>meta</kbd>.

I never need to press <kbd>shift</kbd> along with <kbd>meta</kbd>, which allowed me to free up <kbd>a</kbd>/<kbd>;</kbd> for other uses, where pressing possibly *all* modifiers (except <kbd>meta</kbd>) at once is significant.

So now, my tapholdy <kbd>a</kbd> acts as an arrow keys layer! Along with things like <kbd>pagedown</kbd>, <kbd>pageup</kbd>, <kbd>home</kbd>, <kbd>end</kbd>, <kbd>backspace</kbd>, <kbd>delete</kbd>, <kbd>tab</kbd>, and even <kbd>enter</kbd>.

Back in xremap, I also had an arrow keys layer similar to this one. However, instead of being a *while held* layer, it was a *toggleable* layer, that was much more of a pain to use because even for a single arrow down I'd have to press <kbd>capslock+f</kbd> twice.

But now that I can just hold <kbd>a</kbd> instead of trying to come up with a modifier for which I have no available key, I can easily use the arrow keys layer as a "while held"! \
Absolutely stupid hotkeys like discord's <kbd>ctrl+shift+alt+[down|up]</kbd> and firefox's <kbd>ctrl+shift+[pgup|pgdn]</kbd> are now not just viable, but *easy* to press!

I value remappability in programs so strongly (and it's part of the reason why I [switched to floorp](@/floorp/index.md)) because I refuse to press the really uncomfortable default hotkeys that plague every program. <kbd>ctrl+f</kbd>? <kbd>ctrl+d</kbd>? No thank you. \
But now they're not a problem!

So aside from just making things more comfortable, home row mods also allow me to *chill* about remapping every program I use to make it viable: since I can press anything easily now, I can just stick with the defaults of most things and not worry about it!

Tapholdy <kbd>;</kbd> is my numpad layer. Always hated reaching for the number row, so now I have this neat virtual numpad: 1234567890 = fdsrewvcxa \
Tapholdy <kbd>/</kbd> leads me to the function key layer (F1-F12), and mimics the number layout of the numpad layer. F11 goes on q, F12 on z. Sue me. \
<kbd>e</kbd>, <kbd>i</kbd> and <kbd>'</kbd> are layers for the various "global functionality" hotkeys I used to have in xremap. \
Things that used to be on capslock or the meta key, possibly with extra modifiers, are just a single key away now!

I hate the effort it takes to type various symbols, like @*#`&, etc. This is why some time ago in xremap, I made capslock + the right side keys to output all these symbols, placed how I like! \
The downside is that with a single modifier, I can't exactly place *all* the special characters on the keys that I'd like, and some of them still need to be on an extra modifier aside from capslock, to fit. \
This is why I use <kbd>x</kbd> and <kbd>c</kbd> as my *two* symbol layers!

Now not only do I get to place all of the symbols onto my preferred keys, but I also get a bunch of unoccupied spaces! That's where the next thing comes in.

On linux, you can press <kbd>ctrl+shift+u</kbd> almost everywhere to insert a unicode symbol by typing in its codepoint. \
Kanata has an interesting hacky action called `unicode` that uses this exact mechanism, to allow you to enter unicodes on a whim.

To my great surprise, it actually works and does so consistently! So I made use of the free spaces on my symbol layer, to have them `unicode` other things I commonly use: “”—‼️™

But the concept of the <kbd>x</kbd> and <kbd>c</kbd> layers is still, for the most part, for "special characters" rather than emojis. And I use emojis a lot! \
So that's why I decided to add *another* layer that is specifically for entering emojis. This layer uses the keys <kbd>r</kbd> and <kbd>u</kbd> so that I can comfortably use both sides of my keyboard to assign emoji shortcuts. Here are some of the emojis that are on this layer now: 😭🤯👏✍️🗣️💀🔥🚀🐐←↓↑→😋🥺❌✅

## this deserves a section

Now onto the final tapholdy, the one that fully wraps me in excitement and awe at my decision to switch to 1. kanata 2. every row modifiers. A layer so good that it can make the entire journey worth it by itself. \
Meet the venerable 𝕄𝕆𝕌𝕊𝔼 𝕄𝕆𝔻𝔼

(I just went and made a doublestruck letter layer for this, lol)

I have tried many times to make mouse mode work, and have attempted multiple very different approaches. \
All of them ended up being constantly forgotten about, as none of them are truly *comfortable* to use.

Since I have so many free modifiers, and am interested in making full use of kanata, I decided to try making a mouse mode again. I did not expect for it to be this amazing!

I hold <kbd>Space</kbd> to enable the layer, and that's quite important for reasons I'll explain later. \
<kbd>h</kbd>, <kbd>j</kbd>, <kbd>k</kbd>, <kbd>l</kbd> move the mouse directionally.

For the first 300ms, the moving speed is quite slow. But after that, the mouse moves at a good pace. \
I found that, for better or worse, it's most natural for me to get "more or less there" with the mouse, and then do microadjustments to actually pinpoint the place I want to click. \
By starting slow, I can play into this naturality: I can easily tap multiple times to adjust the precise position, but move plentily fast on hold!

As I mentioned, usually the mouse mode is something I didn't use in normal operation, but instead only when I felt like being extra. But *this* I actually feel like using almost always, over the real mouse!

Initially I went with the obvious <kbd>u</kbd>, <kbd>i</kbd>, <kbd>o</kbd> for the left, middle, right clicks, but during usage I realized something interesting: moving the mouse with the mouse mode feels *so* good, that I can actually consider click-dragging over text to select it... Yes really! \
Holding <kbd>u</kbd> with one of, or multiple of <kbd>h</kbd>, <kbd>j</kbd>, <kbd>k</kbd>, <kbd>l</kbd> feels really uncomfortable, as I didn't design with the thought in mind. \
That's why I put <kbd>f</kbd> to be left click, <kbd>d</kbd> to right click, <kbd>s</kbd> to middle click, and this is also the reason why <kbd>Space</kbd>, rather than my initial <kbd>z</kbd>, is the key that enables mouse mode.

Sounds perfect at this point, right? Now I can move and then click with no interruption, as well as easily be able to click and drag, with any of the three mouse buttons! \
But I got another idea: since I'm already supporting click dragging with all three buttons, why wouldn't I also support holding modifiers while clicking? \
<kbd>s</kbd>, <kbd>d</kbd> and <kbd>f</kbd> would be required for this, so I moved left click onto <kbd>a</kbd>, right click to <kbd>;</kbd>, middle click to <kbd>'</kbd>.

Incidentally, mouse wheel scrolling are ←↑↓→ on <kbd>m</kbd>, <kbd>,</kbd>, <kbd>.</kbd>, <kbd>/</kbd> and the back / forward buttons are on <kbd>[</kbd> / <kbd>]</kbd>

# conclusion

It took me around 3 days to get comfortable using this entire system. I might be spotty at some hotkeys here and there, but normal operation feels quite solid already. \
This sounds like a pretty fast learning curve to me!

Not having to cry over horrendous hotkeys, or over keys being too far away to be viable to press, is truly heavenly. \
If you haven't tried every row mods, and **especially** if you have and gave up, I encourage you to apply my approaches and tips to make this system viable for yourself too. It's a lifechanger!
