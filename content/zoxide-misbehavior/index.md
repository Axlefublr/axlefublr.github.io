+++
title = 'zoxide misbehavior'
date = '2025-06-08'
+++

I just found a very surprising behavior zoxide gives fish users, that might be part of the reason why zoxide always felt so useless to me in the past.

I noticed that one of my directories spiked up in its weight every so often; consistently it would get to the maximum, 9999, while I for sure didn't zoxide to it nearly that often. \
The reason why this is an issue, is because of how zoxide cleans directories: \
If a directory's weight is too small, *compared to the highest directory*, the directory may get removed from the zoxide database, being considered unimportant. \
But I don't even need the spiking directory to be zoxided at all! I navigate to it using a different method of mine, that I'm currrently writing another blog post about. \
So having it destroy the other directories, that I **do** heavily want zoxided, is quite problematic.

I looked through all my config, and all the places where I use the `z` alias — none of them could've led to me spiking that directory. \
The directory I'm talking about is [`magazine`](@/magazines.md) — every time I modify a file in that directory, a fish function gets ran that *`cd`s into the directory* and makes a commit. \
I made sure that it uses specifically `cd` rather than `z` when I made it, because obviously I don't want to spike a directory due to a non-interactive, automated action.

So I decided to look into what fish code the below line in my config actually executes.
```fish
zoxide init fish | source
```

My assumption was that it gives me the `z` alias, which increments directories I travel to *using it*. \
But imagine my terror when I realize that it's not the semantic it actually uses!

`zoxide init fish` actually installs a hook on the `$PWD` variable; \
Every time your current working directory (`$PWD`) changes, that directory is incremented. \
There is nothing about the `z` alias in specific that increments directories — *any* method of getting to them does!

That is in theory nice, but in practice it easily makes various directories spike up unfairly; if you do any sort of automatic / non-interactive scripting, you might be affected by this.

The simple fix for this issue is to do the following instead:
```fish
if status is-interactive
  zoxide init fish | source
else
  alias z cd
end
```
If you are in an interactive shell session, initialize the hook and give me the `z` alias. If not, make `z` just act like `cd`.

This is pretty good, but leaves a poor taste in my mouth. \
The semantic in my brain is: “If I'm using `z`, that's because I want to increment the directory. If I don't, I use `cd`.” \
I very much may want to zoxide increment some directory non-interactively too!

A simple usecase I can immediately think of, is some script that sets up all active projects I'm working on, and zoxides into all of them so that I can jump to them later more easily. \
If I can come up with a genuinely viable usecase like that on the spot, I'm sure that there will be other situations where I don't even realize I want to zoxide! \
I feel it's much better to just carry the simpler semantic of “`z` to increment, `cd` to not”, rather than “`z` to increment, unless it's not an interactive session, …”.

Here's what I put into my config, to achieve that:
```fish
zoxide init fish | source
function __zoxide_hook
end
function __zoxide_z
    set -l argc (builtin count $argv)
    if test $argc -eq 0
        __zoxide_cd $HOME
    else if test "$argv" = -
        __zoxide_cd -
    else if test $argc -eq 1 -a -d $argv[1]
        __zoxide_cd $argv[1]
        command zoxide add -- (__zoxide_pwd)
    else if test $argc -eq 2 -a $argv[1] = --
        __zoxide_cd -- $argv[2]
        command zoxide add -- (__zoxide_pwd)
    else
        set -l result (command zoxide query --exclude (__zoxide_pwd) -- $argv)
        and __zoxide_cd $result
        and command zoxide add -- (__zoxide_pwd)
    end
end
```

Notice how now the `| source` line runs *always*, regardless of if I'm in an interactive session or not. \
Then, I disable the `$PWD` variable hook. \
Doing so makes `z` practically just act like `cd`, so to give it back the power of incrementing, I add in `command zoxide add -- (__zoxide_pwd)` in the branches where incrementing makes sense.

In the future, zoxide devs might update the `__zoxide_z` function to make it better in various ways; So if you're reading this blog post in the future, let me explain how I figured out which function to even modify, so that you can *recreate it*, rather than copy a possibly outdated version directly above.

First, do `type z` — this will show you that `z` is a fish function, that calls `__zoxide_z` inside of it. \
Now, do `funced __zoxide_z` to open the function's source in your editor; Copy it to your clipboard so that you can blammo it in your `config.fish`. \
Add the `command zoxide add -- (__zoxide_pwd)` to the various branches similar to how I do above.

To find the function that sets up a hook in the first place, I simply piped `zoxide init fish` into my pager, and looked through the code. I noticed `--on-variable`, and the rest is history.

The reason why we aren't just modifying the functions directly with `funced` (which you can usually do), is because they're defined dynamically every time you run your shell, rather than being `funcsave`d and staying as a file. \
So if we were to edit the functions directly, they'd only stay modified for the current session, rather than all future sessions. \
That's why we need to blammo directly into our config.

Now that things are working correctly, I can finally *trust* zoxide to work as expected. \
I won't have to worry about initialized directories randomly disappearing, and with [another thing I did](@/optimizing-paths/index.md), my zoxide is overpowered now.

🍐🚀
