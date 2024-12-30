+++
title = 'suspend is actually neat'
date = '2024-12-31'
+++

Part of why [I still use kitty](@/why-I-hate-kitty/index.md) is because its window manipulation is goated. \
You can create windows (panes), tabs, overlay windows (panes) and os windows using hotkeys. \
That's cool enough on its own, but you can do all of that from a *script* too!
This lets you integrate window manipulation into a bunch of places, so it's quite workflow-building.

Say I'm in helix, and want to run some shell command.
I can of course use `:sh`, but it's pretty cumbersome, so I'd rather use my actual terminal.

Not only can I *just* create a new pane, I can create it in the same cwd as the current one, or as the same cwd as *helix* is in ([my helix fork](https://github.com/Axlefublr/helix) exclusive feature), or in the parent directory of the current helix buffer (helix fork specific also).

This isn't a post on why kitty is amazing, because as I expressed in the past, I *hate* kitty.
I more so want to shed light on that my window manipulating workflow is super solid, so there was never a need to look into the shell feature called "suspending".

Plus, it's kind of confusing and last time I tried it, it felt wonky.

{{ hr(id="yoink")}}

I happened to be reading a [blog post](https://jvns.ca/blog/2024/07/03/reasons-to-use-job-control/) by Julia Evans some time ago, and it goes on to explain the benefits of actually using job control. \
(It was a great read but) at this point I don't really remember a single point; all I remember is that "job control *can* be kinda wonky but it's also super neat".

I decided to give it another try. \
While in helix for example, instead of opening another pane to run a shell command (only to close the pane right after), I would instead *suspend* helix and execute the command in the shell that I already had open.

Very very quickly I realized that I absolutely love it!

# command output

When I create a new pane to run a shell command, I'll probably close it right after. \
Having windows I forgot about *open* feels messy, after all.

But when I close a pane, naturally I lose all the output of the commands that I ran previously.

This doesn't happen with a suspending workflow!
The output of previous commands actually *stays* and I can look at it again as many times as I want to.

Better yet, I can switch back and forth between helix and the shell output very quickly — as fast as I would be switching between multiple (fullscreened) windows.
(But with the benefit of not needing to clean anything up)

This factor really started to shine once I was fucking around with colors using `pastel`. \
I couldn't quite get the correct lightness factor on some color, and so I was going back and forth between helix and my shell, using the previous color (displayed in the output) as the input to a new `pastel darken 0.01` command; then, going back to helix to try out where I was actually using the color.

This worked so amazingly for me because I never felt like I needed to clean up a pane after I'm "done". \
With a pane, I might've closed it early and lost output that I then would realize I need, a couple of seconds later. \
And in the same fashion, keeping multiple windows open feels quite bloaty, and I see it disrupt my worklow in a way that suspending simply doesn't.

# toggle hotkey

Another thing that helps a lot in the workflow, is how natural it is to make a hotkey for it.

With a pane, you need a hotkey to *create* a window, and a different hotkey to switch between windows.
Then maybe even another hotkey to switch windows *backwards*.

I don't need that with suspend! \
The hotkey is simply <kbd>ctrl+z</kbd> to suspend; and I made a hotkey in my *shell* to run `fg` — that puts the latest job into the foreground.

I do something in helix and realize I need to run a command.
1. <kbd>ctrl+z</kbd>
2. run command
3. <kbd>ctrl+z</kbd> *again*, now to go back to helix
4. FIN

Rather than:
1. hotkey 1 to create a new pane
2. run command
3. hotkey 2 to close it
4. FIN?
5. oh fACK I needed that output, now I have to repeat this process to get it back

{{ hr(id="strong") }}

Neither of these are particularly *that* strong of arguments; ultimately this suspending workflow simply felt *neat*. \
So try it out, maybe you're be as overjoyed as I am!

However, I was still missing a part of my windowing workflow power. \
As I mentioned, I can do something like "create pane in the working directory of *helix*" or "... in the parent directory of the current helix buffer".

The pane creating hotkeys know about this info because I explicitly pass it; how would suspend know about it?

By me *forcing* it to know! 😈

{{ hr(id="force") }}

In my helix fork, I made the suspend action write the current buffer's parent directory to a path, and the current helix cwd to a different path. \
Then for yazi, I wrote a small plugin that writes the current directory path to both of those files every time I change directory.

Now, I have a hotkey in my shell on <kbd>ctrl+y</kbd> that toggles between the two paths listed in those two files.

So now, instead of needing to create a new pane in some specific directory with dedicated mappings, I can *retroactively* cd into the directory from which I want to execute the command.

I'm working on a new script in helix, and want to run it. \
I suspend helix, and press <kbd>ctrl+y</kbd> to switch to the parent directory of that script, and simply do `./script.fish`. \
I press <kbd>ctrl+y</kbd> only once, assuming that my shell's cwd and *helix*'s cwd match up; \
if they didn't, I'd have to press <kbd>ctrl+y</kbd> twice.

> Note that my <kbd>ctrl+z</kbd> is actually on <kbd>capslock+w</kbd> and <kbd>ctrl+y</kbd> is on <kbd>capslock+e</kbd> \
> So these two hotkeys are actually way more ergonomic to press than the *real* <kbd>ctrl+z</kbd> and <kbd>ctrl+y</kbd> would be

Me storing helix's cwd solves the other part of of the windowing workflow — "create pane in the cwd of *helix*". \
Except, it does it *better*.

With the windowing workflow, I need to proactively think about how to create my new window. \
"Which cwd do I need?"

Even if I just default to helix's cwd, I'm bound to have to use a different mapping to open windows from helix, than I usually would anywhere else. \
And even then, I still need to proactively think about creating windows in the parent directory of a buffer.

The suspend workflow wins here yet again, because I *fix* my current working directory, instead of having to *prepare* it.

The thought process in this workflow is way simpler.
1. I need to execute a command
2. suspend
3. *starts writing command*
4. "oh right I need to execute it from the parent directory of that buffer"
5. *presses <kbd>ctrl+y</kbd>*
6. *continues writing command and executes it*

I don't even need to *delete* the command and then re-write it; the cwd fix is on a hotkey! \
So this process is *incredibly* smooth 😋

{{ hr(id="hope") }}

I hope I was able to show you how a suspend-driven workflow can work really well! \
Try out using job control more, maybe you will discover that you love it too!
