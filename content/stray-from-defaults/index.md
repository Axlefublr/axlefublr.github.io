+++
title = 'stray from defaults'
date = '2025-01-02'
+++

Vim has made a stupid design decision, and we're still eating shit many years later.

It prioritised mnemonics, rather than ergonomics.

The issue is pretty apparent: mnemonics are helpful for the first, like, *week* while you're learning vim for the first time. \
And then many *years* later you still end up using the same non-optimal mappings.

{{ hr(id="the-mappings") }}

One of the most frequent things you do in a text editor, is copy. \
And yet it's mapped to one of the most annoying keys to press: <kbd>y</kbd>. While powered by a big stretch of a mnemonic that we just came to accept: "yank".

And yet <kbd>s</kbd> sits there undisturbed, in one of the most prime finger positions on the home row, doing what `cl` can also do.

> Don't get me wrong: I loved using <kbd>s</kbd>, but it is too convenient of a placement.

I hate pressing <kbd>p</kbd> almost as much as pressing <kbd>y</kbd>. \
It is another example of an 😩 key used for one of the most common actions. Except, pasting is probably more common than copying.

<kbd>o</kbd> is one of the *nicest* to press keys, yet it's there for a fairly niche action: creating a new line.

What I find myself doing the most after creating a new line, is to create *another* line. \
Because <kbd>o</kbd> puts you into insert mode, the natural way to create another line is by pressing enter. \
Yet, the hand motion of switching from <kbd>o</kbd> to <kbd>Enter</kbd> is pretty cumbersome.

What is the most common usecase for <kbd>A</kbd>? \
In my experience, it's to add in a `;` or a `,`. \
However, if you correctly use both shifts, the hand movement to do an action like this is a bit too ridiculous: for such a *common* usecase, having to move your pinky *aaaaallllll* the way from the right shift to `;` ends up compiling into an annoyance.

The choice of <kbd>a</kbd> is also strange in a different way. \
To insert at the *left* of the cursor, you press the *right* key (<kbd>i</kbd>). To insert at the *right* of the cursor, you press the *left* key (<kbd>a</kbd>). \
???

# you can stop eating shit

Defaults are made to be CHANGED from. \
It's great when they are good to begin with, but that's not always the case. \
Do not be afraid of *change* when it can provide you a much nicer experience.

My copy key is now <kbd>s</kbd>. \
What's the mnemonic? "Steal"? \
Wrong question. I don't *need* a mnemonic! If the mapping *feels* right, your muscle memory adjusts pretty quick!

Now I don't have to travel across the Alps just to copy something: the key is on my home row.

My paste key is <kbd>o</kbd>. \
*Much* nicer to press than <kbd>p</kbd>! And in addition, something like `yyp` is now `sso`, which I find to simply be much more enjoyable.

Insert new line is now on <kbd>a</kbd>. \
Now when I insert a new line, the motion of my right hand to reach <kbd>Enter</kbd> is much smoother! \
Smoother even if I do "insert new line above" (<kbd>A</kbd>) first; all I need to do is slightly move the right pinky from right shift to <kbd>Enter</kbd>.

Insert *after* the cursor is on <kbd>p</kbd>. \
This was risky in my head, because I don't *like* pressing <kbd>p</kbd>. But very quickly I realized just how much nicer it is than <kbd>a</kbd> for the purpose.

First of all, it *is* on the right side of <kbd>i</kbd>! So, <kbd>i</kbd> and <kbd>p</kbd> now play along with a consistent semantic of "this one is on the left, this means *before*" and "this one is on the right, this means *after*".

But even better than that, appending is AMAZING now! \
The hand movement to append a `;` or `,` at the end of the line is way smaller now, and feels really comfortable. \
Also generally, pressing right shift is more difficult than pressing *left* shift anyway; so overall there's less hand movement for something that I do really frequently, especially while writing a blog post for example.

{{ hr(id="conclusion") }}

After changing these, it took me *maybe* two days to get used to the new mappings. This is simply because of how much more *fitting* they feel in my head, and to my fingers.

You will have your own gripes that you will solve differently than me, but I wanted to shed light on that it *can* be **really** beneficial to change defaults, even when they are as strong-rooted as the ones I listed.

It's true: a lot of *other* programs will replicate vim, and have semantically fitting `hjkl`, `y`, `p`, etc mappings. \
What do you do about that?

Personally, I just context switch, or reconfigure the mappings.
This may seem like a lot of effort, or a non-ideal solution, but look at it from a different lens: why *would* you sacrifice your experience in the place where you spend most of your time (your editor), just for some *other* program to feel consistent?

I do agree, context switching is annoying, but can be learned: if you have a strong thought of "this is a different program", you can eventually stop caring that its mappings are inconsistent with what you remapped.
Or, you know, remap them in that program as well.
