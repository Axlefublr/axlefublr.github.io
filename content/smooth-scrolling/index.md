+++
title = 'smooth scrolling'
date = '2025-09-16'
draft = true
+++

I tried out Zen Browser recently, and one of the many good things about it that I noticed, is how *smooth* the scrolling feels. \
I thought: “surely it's not zen-specific!” and turns out that its smoothness is due to `about:config` settings you can set in *any* firefox :D

The biggest / most important change, that will make your scrolling a *lot* smoother, is setting `general.smoothScroll.msdPhysics.enabled` to `true`. \
Honestly, I'm not sure why it isn't true by default! (in non-zen firefoxi)

But of course, I didn't stop there. Let's take a look at the other relevant settings. \
First of all, I'll point out that the *other* `general.smoothScroll.*` settings, that aren't in the `msdPhysics` section, no longer apply — so you can pretty much just ignore them.
Unsure if that's intentional or a bug honestly; I remember seeing some issue that claimed this to be a bug 👀

Anyway! \
There are two quite core settings:
```toml
mousewheel.default.delta_multiplier_x = 100
mousewheel.default.delta_multiplier_y = 100
```

Pretty self-explanatory — these two multiply the horizontal / vertical scrolling speed. \
The *baseline* (but not necessarily the default) is 100 — you can come back to these later to more directly express “please faster” and “please slower”. \
I ended up sticking with 100 for both, however.

```toml
general.smoothScroll.msdPhysics.motionBeginSpringConstant = 300
general.smoothScroll.msdPhysics.regularSpringConstant = 900
general.smoothScroll.msdPhysics.slowdownSpringConstant = 300
```

“Spring” is an animation type. \
The constant here is somewhat of an arbitrary number, in not sure which range, but the main idea is that lower is more draggy / slow / smooth, and higher is more abrupt / fast / jumpy.

By setting the begin and slowdown springs to a lower number, 300, both starting and ending a scroll feels very *smooth* and gentle, without being *too* slow. \
The *regular* constant, though, I have no clue does what!
At least, I wasn't able to find a notable difference in my experimentation. \
But I set it to 900 for it to feel more “tight”

```toml
general.smoothScroll.msdPhysics.slowdownMinDeltaMS = 12
general.smoothScroll.msdPhysics.slowdownMinDeltaRatio = 1.3
```
These two I never quite got do what, either.

```toml
general.smoothScroll.msdPhysics.continuousMotionMaxDeltaMS = 3
```
This, as far as I understand, basically means “every how many milliseconds is the smooth scrolling logic calculated?” \
It's 12 by default, so 3 is a lot more smooth! At least by an eye test.

{{video(path="showcase")}}

Here's how all these settings applied look like! \
Keep in mind, there might be some fps loss in the recording, so it likely looks even smoother irl.

[The blog post I was scrolling](https://jdugan6240.dev/posts/python_a_beautiful_mess).

# mouse mode

I don't actually use my real mouse when scrolling, most of the time; \
I have two keys in my [mouse mode](@/erm/index.md) that send <kbd>WheelUp</kbd> and <kbd>WheelDown</kbd>, so that's how I scroll most of the time.

Did a bit of an optimization on the side of my mouse mode, to make scrolling even smoother.

```
o (mwheel-up    30 60)
i (mwheel-down  30 60)
w (mwheel-left  20 60)
r (mwheel-right 20 60)
```

The default “length” of a singular scroll (a nudge?) is 120 — it's noticeably more jumpy than the 60 I'm now using. \
To account for the loss of speed, I send the nudges more frequently: 30 instead of whatever value I used to have prior. \
*This* is what you're seeing me use in the recording, btw!

Kanata does warn me, though, that setting the length to something else than a multiple of 120 is error-prone — some programs might get utterly confused. \
I already came across an example of that happening, with [oculante](https://github.com/woelper/oculante); but it wouldn't be the last issue oculante has anyway :p \
So I'll gladly optimize for the #1 place where I scroll — my browser 😌

# acceleration

There are another two settings that are interesting to play with.
```toml
mousewheel.acceleration.factor = 2
mousewheel.acceleration.start = 20
```

Start is “after how many nudges do we increase the scrolling speed?” and factor is “by how many times?” \
Using these two, you can make it so you start to scroll *faster* after some amount of continuous scrolling. \
Unfortunately factor has to be an integer :< so we can't speed up just slightly.

I ended up going without this feature (setting start to `-1`), but it might work for you 😼
