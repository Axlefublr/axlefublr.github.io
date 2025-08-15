+++
title = 'i overuse anki'
date = '2025-08-15'
draft = true
+++

No, I have not burned out on anki. \
All the advice I share in my [how to anki](@/how-to-anki.md) blog post still works… \
A little too well, actually??

A very common issue anki users face is that your workload creeps up on you over time, constantly increasing and eventually burning you out. \
My system, however, is designed specifically to not allow that! \
What is “it only seems free!!” for others, *is* quite free for me.

What this resulted in, is that I don't take notes **at all**. \
I either make an anki card, or discard the information. \
This has been working really well as I've already expressed, but has a more *hidden* caveat I noticed only recently.

{{hr(id="quickshell")}}

I want to make myself an analog clock :3 \
Maybe a window, maybe an overlay widget; \
Some sort of easily and quickly openable gadget that would allow me to *feel* the time.

Istg I've never been to the military, but I can only *feel* time on an analog clock; \
Digital ones *tell* me the time, but they don't *show* me it, yk?

In my fairly short adventure of trying to find an analog clock thingymabop, I haven't found anything that I actually feel like *using*.

TUI clocks are god awfully ugly and therefore impractical — too cell dependent to be graphically accurate and neat-looking.

The browser is often a common platform to build guis in, and quite importantly *easy* to build guis in. \
I found [a project](https://github.com/abhishekgurjar-in/Analog-Clock) that is a basic analog clock website, then [forked it](https://github.com/Axlefublr/annabirch) to make my changes. \
I quite like it!

![](./annabirch.webp)

However the issue is that it *is* in my browser. \
Something I'd like to be able to do is look at the time, ad-hoc, while doing anything else at the time. \
If, say, I'm watching a youtube video in fullscreen, and then open the clock in a new tab, well now I've unfullscreened my video and it's a bit unpleasant :/ \
If I force open it in a new browser window, it's now **slow** to open!
And that's no good...

So I need some sort of “just a window” clock app / program, that opens **fast** and is still styled how I like it to be. \
I found [this project](https://github.com/Depau/wlclock) which might actually fulfill my needs, but I need to edit C++ code to customize it. \
Some very basic things are defined as uhhhhhh C header variables? (idk, don't ask me), but to make it be how I want it, I need to edit the normal source too. \
Somehow, in all my trying, I could **not** for the life of me figure out how to make the background of the app fully transparent. \
Funny thing is, the *sides* are already transparent, suggesting that it *is* something the program is able to do, but there's a square background defined god knows where, configured by god knows what!
And I cannot change it 😭 At least, I don't know how...

I was meaning to get back into trying to figure this out, after initially failing, but now I'm feeling like it's a bit of a fool's errand. \
How about I employ a fun learning trick I like to use?

Instead of focusing *only* and **exactly** on my specific usecase, I'll use that problem as a motivator to go learn something larger.

I want to make hotkeys that use `niri --json` to do various windowing actions — cool, I'll learn nushell as it lets me operate on json conveniently, *among many other things*. \
Would be cool if [floorp](@/floorp/index.md)'s wrapped tabs were all of equal size, rather than the last row expanding the tabs on it — epic, I'll learn css grid and only *after* realize that the idea was not feasible{{fn(i=1)}} \
I want an analog clock — nice, I'll learn *quickshell*.

How exciting!
New learning project :D \
I start reading quickshell's docs, and open the docs of QtQuick… Only to find myself in **terror**. \
There is **so much**!

So many pages, so many thingies, *so much to learn*. \
And all of this somooch (💋) will end up as anki cards, maintained in my memory forever, only for me possibly to never use like, 90% of it. \
There's a good chance I'll actually never get to making the clock, and leave after making a volume change osd. \
Yet, despite not being used information, I'll remember it until the end of time...

THAT is when I finally noticed, that the *volume* of how I use anki, actually *restricts* me.
❗cards need to catch up to what I've learned before I learn the next thing

# footnotes

{{hn(i=1)}} I'm not mad about this btw — the main goal was to learn css grid, but hide the effort from my psyche with the promise of something really cool. \
I didn't get that something cool, sure, but by the time I finish learning, the urgency of the motivator has already died down — it was *used* as coal for the fire of learning 🔥

❗limits the volume of new information I allow myself to consume — newly learn things carry a chain and balls of eventually having to be memorized in their entirety. some things really do not deserve it, and I cannot know ahead of time what I'll be needing all the time, and what is a waste of my memory
❗this time and energy spent on remembering, is not spent on learning the next new thing, and results in a very “spiky” (not rounded) knowledge
❗wanted to create analog clock, eww quickshell eww, very tea with honey without honey no tea /ref to kingbach
❗the optimize directories blog post has made it a lot more viable to navigate to directories, and therefore to have multiple for different usecase, it's part of how making the two noties directories is viable
❗splitting via semantic: tiq is what would be a magazine but would be a waste, mdw is more documentation-like
