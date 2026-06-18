+++
title = 'task scheduler'
date = '2026-06-18'
+++

I've been using this neat task scheduling program called [pueue](https://github.com/nukesor/pueue) for a while.
With it, you can queue up shell commands to be executed in sequence, and/or in parallel, without “living” in a terminal window.

After [my recent niri ricing improvements](@/nirism/index.md), the no-terminal-required benefit isn't as large anymore.
If I want some terminals out of my way, I can just throw them onto a workspace and bob will be my uncle, pretty much.
Then, I don't really use parallelism in pueue much — generally, I just want everything executed sequentially.

The *workflow* is part of what annoys me with pueue, I think.
If the command you schedule to execute finishes with no issues — fantastic.
But if it fails?

You blammo a `pueue status`, to figure out *which* task failed.
You then extract its id to put it into `pueue log <id>`.
Ah and don't forget to `-f` to actually show the whole output.

This... is kind of annoying.
When something fails, I want to immediately jump to *the* place that failed, see the entire output, and then be able to decide on the spot what to do about the failure.
I thought creating a pueue interface thingy would help soothe my concerns, but the inherent complexity of pueue piled up on me quickly with no clear benefit.

So I thought: how about I create my own system?
I use very few pueue features, so by limiting the scope I can make my system simple but effective.

The main idea is the following: I have a terminal on one of my workspaces, that is launched on login.
The terminal is permanently executing a script which takes shell commands (from somewhere) as input, and executes them as it gets them, sequentially.
At the start of executing, the entire command is printed (really important for when things go wrong!).
When it finishes without problems, I see a green “success” and a notification.
If it didn't succeed — a red “failure”, once again a notification, and a *prompt* that asks me what to do next.

When there's a window that exists **entirely** to show me how my tasks are going, checking when things go wrong becomes so much more pleasant.
Rather than creating the pueue status window, locating the id, forgetting to include `-f`, and snoozing 😴, \
I can just press my <kbd>g↓u</kbd> hotkey to activate the executor window, decide what to do, and press <kbd>g↓u</kbd> again to go back to whatever I was doing.
Incredibly pleasant!

Additionally, checking *progress* is fantastic.
Downloading videos from youtube with [yt-dlp](https://github.com/yt-dlp/yt-dlp) is a common usecase of mine, for pueue.
But checking how much has downloaded so far feels like enough of a *process* that I don't feel like doing it...
But if it's just two hotkey presses to see the latest output, that's pretty nice! :D

# client

I'll be using a named pipe{{fn(i=1)}} to pass data from the “client” to the “server”.
Named pipes are a special type of “file”, for which read and write rules work a bit differently.
Whenever you read from a named pipe, the content you read is no longer stored in it — it's essentially consumed.
And notably, the *writing* side must wait for what it wrote to be read (consumed), before the writing operation can finish.
So generally, you have one side that indefinitely continues reading from the named pipe, to make sure it never gets “clogged”, and then you write into the named pipe however you wish, it being not much different from writing into a normal ass file.

Let's start with the scheduling script, which will be called by the client.
Below is `schedule.fish`:
```fish
#!/usr/bin/env fish

begin
    echo -n "$argv"
    echo -ne '\0'
end >~/.local/share/mine/task-scheduler
```

The script takes arguments (or just one), which is the shell command that you want to execute.
It then writes it into the named pipe.
This could be done manually for the record, it's just convenient to have a script for this, due to some extra developments we'll make next.

It ends the command with the *null* character, to support multiline shell commands.
Although feeling like I need to support that comes from a usecase that I'll solve differently — we'll get to that in due time.

Scheduling a command to execute will look something like this:
```fish
schedule.fish ffmpeg -y -i "$input" $from $to \
   -vf "'pad=ceil(iw/2)*2:ceil(ih/2)*2'" \
   $fake_audio \
   -c:v libx264 \
   -preset medium \
   -crf 18 \
   -map_metadata -1 \
   -movflags +faststart \
   -c:a aac \
   -b:a 128K \
   -shortest \
   "$output"
```

This scary-looking command is from one of my scripts, where I used to use pueue for the scheduling; but now I get to use my own system 😌
I use a couple of variables in the command that I'm scheduling — they get expanded into literal arguments before actually being sent to the server.
So the server doesn't need to resolve `$fake_audio`, `$input`, `$output`, etc: it just handles executing the already expanded shell command.
Thanks to this, I don't have to come up with a fancy system of providing the client's environment variables into the server.
I started developing that functionality, but soon realized that I don't need it that much 🤷‍♀️
And if I do, I can always explicitly use `env`.

One piece of environment I *do* want to provide, though, is the current working directory from which the client wanted to execute the shell command from.
Matters a lot for relative paths.
Let's edit the scheduler to pass this data as well:
```fish
#!/usr/bin/env fish

argparse D= -- $argv

begin
    if set -q _flag_D
        echo -n $_flag_D
        echo -ne '\0'
    else
        echo -n $PWD
        echo -ne '\0'
    end

    echo -n "$argv"
    echo -ne '\0'
end >~/.local/share/mine/task-scheduler
```

Now, as a single write, we provide the working directory and the actual command; both terminated by the null character.
Makes parsing it back out in the server side easy.

By default, the working directory we pass is just the directory that the client (for example my terminal where I'm calling `schedule.fish`) is in.
But I can also pass a different directory using the `-D` flag.
Thanks to `fish`'s `argparse`, we get the functionality of `--` for free, so we don't have to worry about the command we're passing possibly also having a `-D` flag.

# server

Here similarly, I'll unveil the full script step by step.
This is the core idea of `receiver.fish` — the “server”.
```fish
#!/usr/bin/env fish

mkfifo ~/.local/share/mine/task-scheduler 2>/dev/null
tail -f ~/.local/share/mine/task-scheduler | while read -z first
    and read -z second
    cd $first
    while true
        fish -c $second </dev/tty
        if test $status -eq 0
            break
        end
    end
    cd ~
end
```

We make sure the named pipe actually exists, and then start indefinitely reading from it with `tail -f`.
Every read consists of two items, that are terminated with a null character: the current working directory, and the actual command to execute.

We `cd` into the directory we were given, so that we execute the provided command from *there*.
Later we reset back to the home directory.

The command reruns until it succeeds (for now), and we use the `</dev/tty` trick to fix interactive input for the command.
Without it, if I tried to execute `read` for example, it'd just instantly fail because the command is not connected to the input of the terminal that's running the server script 🧐

This already works!
But could be nicer 😌
So let's make it nicer!

First, it's actually pretty useful information to know what directory the command is executed in.
But only if it's *not* executed from home (`~`), cause that I can assume.
Let's show the directory (with `/home/axlefublr` folded into `~`) before cding to it.

```fish
#!/usr/bin/env fish

mkfifo ~/.local/share/mine/task-scheduler 2>/dev/null
tail -f ~/.local/share/mine/task-scheduler | while read -z first
    and read -z second
    if test $first != ~
        set_color -o b58cc6
        string replace -r "^$HOME" '~' $first
        set_color normal
        cd $first
    end
    while true
        fish -c $second </dev/tty
        if test $status -eq 0
            break
        end
    end
    cd ~
end
```

What's even more useful, is seeing the *command* that will be executed.
```fish
#!/usr/bin/env fish

mkfifo ~/.local/share/mine/task-scheduler 2>/dev/null
tail -f ~/.local/share/mine/task-scheduler | while read -z first
    and read -z second
    if test $first != ~
        set_color -o b58cc6
        string replace -r "^$HOME" '~' $first
        set_color normal
        cd $first
    end
    while true
        set_color -o e491b2
        echo $second
        set_color normal
        fish -c $second </dev/tty
        if test $status -eq 0
            break
        end
    end
    cd ~
end
```

Some commands don't produce any output that would make it obvious the command finished.
So let's print a nice green “success” when a command successfully finished, and a scary red “failure” when it doesn't.
```fish
#!/usr/bin/env fish

mkfifo ~/.local/share/mine/task-scheduler 2>/dev/null
tail -f ~/.local/share/mine/task-scheduler | while read -z first
    and read -z second
    if test $first != ~
        set_color -o b58cc6
        string replace -r "^$HOME" '~' $first
        cd $first
    end
    while true
        set_color -o e491b2
        echo $second
        set_color normal
        fish -c $second </dev/tty
        if test $status -eq 0
            set_color -o a9b665
            echo success
            set_color normal
            break
        else
            set_color -o ea6962
            echo failure
            set_color normal
        end
    end
    cd ~
end
```

I will probably not be staring at the executor window as it's doing its thing.
The entire point is to not have to, after all!
So let's add a notification on the command finishing, successful or not.
```fish
#!/usr/bin/env fish

mkfifo ~/.local/share/mine/task-scheduler 2>/dev/null
tail -f ~/.local/share/mine/task-scheduler | while read -z first
    and read -z second
    if test $first != ~
        set_color -o b58cc6
        string replace -r "^$HOME" '~' $first
        cd $first
    end
    while true
        set_color -o e491b2
        echo $second
        set_color normal
        set -l first_word (string split ' ' $second)[1]
        fish -c $second </dev/tty
        if test $status -eq 0
            set_color -o a9b665
            echo success
            set_color normal
            notify-send "$first_word ✅"
            break
        else
            set_color -o ea6962
            echo failure
            set_color normal
            notify-send "$first_word ❌"
        end
    end
    cd ~
end
```

Showing the *entire* command in the notification sounds like the obvious pick, but have you seen the ginormous `ffmpeg` command I exampled above?
I don't wanna see all that.
So, I'll start by just showing the command part of the command in the notification, and in the future see if that's enough or too inspecific.

{{hr(id="error-handling")}}

There's a reason we re-print the command we're executing every time we try to.
Let's add some interactive error handling!

When a failure occurs, I want three different options:
1. run the command again with no changes
2. ignore the failure of this command, continuing to the next command in line
3. edit the command *in my editor*, and then run **that** one

Whenever I want to ask myself of some options, I use a custom program I made some time ago: [confirm.rs](https://github.com/Axlefublr/wks/blob/main/src/bin/confirm.rs){{fn(i=2)}}.
I pass a list of options, and embed the key to press to accept them, into the argument itself.
For example, `a[c]cept` will be picked if I press <kbd>c</kbd>.

Confirm.rs doesn't allow you to press any keys that don't pick an option.
Well, it *does*, it just ignores them.
So you don't have to be particularly careful!

Once you do press a valid key, it's printed to stdout, and can then be compared in a if-else chain to figure out what to do next.
This program exists to be used in shell programs, for the most part.
Very common usecase of mine, to want to present myself with a list of options I want to pick from with a *single* (no pressing <kbd>Enter</kbd>!) key.

The program is in a bit of an inconvenient format (for you) — it's not in its own repo, not on crates.io, no precompiled binaries, undocumented, etc etc.
But for me? GOD is it convenient.
Check out [this blog post of mine](@/rust-scripting-pt-2/index.md) to learn how this system allows me to create ad-hoc rust programs a lot easier.

For the action to edit the command in my editor, I *could* write my own thing as well, but there's no need — a neat program called `vipe`{{fn(i=3)}} lets you pipe input into it, edit it in your `$EDITOR`, and output it back to stdout. So we're gonna use it!

```fish
#!/usr/bin/env fish

mkfifo ~/.local/share/mine/task-scheduler 2>/dev/null
tail -f ~/.local/share/mine/task-scheduler | while read -z first
    and read -z second
    if test $first != ~
        set_color -o b58cc6
        string replace -r "^$HOME" '~' $first
        cd $first
    end
    while true
        set_color -o e491b2
        echo $second
        set_color normal
        set -l first_word (string split ' ' $second)[1]
        fish -c $second </dev/tty
        if test $status -eq 0
            set_color -o a9b665
            echo success
            set_color normal
            notify-send "✅ $first_word"
            echo
            break
        end
        set_color -o ea6962
        notify-send "❌ $first_word"
        confirm.rs failure '[r]estart' '[e]dit' '[k]ancel' | read -l response
        set_color normal
        echo
        if test $response = k
            break
        else if test $response = e
            set second (echo $second | vipe --suffix fish)
        end
    end
    cd ~
end
```

If we press <kbd>k</kbd>, we break out of the loop, essentially skipping the command we're currently retrying infinitely (due to the `while true`). \
If we press <kbd>e</kbd>, we change what the command even *is*, so that the next loop iteration executes something different in the `fish -c $second`. \
If we press <kbd>r</kbd>, the if → else if chain after the confirm.rs *doesn't* catch it, and we reach the end of the loop, restarting executing the command without changing anything.

One extra thing I did, is add printing extra blank lines with a plain `echo` so that visually separating between different commands is even clearer.

{{hr(id="done")}}

The task scheduler is now done!
Now instead of using `pueue add` in my various scripts, I can use `schedule.fish` instead; yet achieve a much nicer user experience.

{{video(path="task-scheduler-done")}}

There's a reason I can't use this for running *every* long-running command, though.

In fish, I can create a hotkey that blammoes my commandline as a singular argument to `schedule.fish`, which is great but has a massive caveat: it *won't* expand the variables to their real values. The server will receive the command raw, and will try to resolve the variables — but it most likely doesn't know about them.

The biggest usecase that showed itself very quickly is my [blammo](@/blammo/index.md) concept.
The `in` variable in fish contains the current / selected files, if I open myself a fish from helix or yazi.
I can then do something like `magick $in (path change-extension .webp $in)` for example.

`in` is a local variable set by my config.fish, so not only is it not known by the server, but it also would be outdated if it *was* known.
And that's just one variable! Specialcasing for *just* that one is silly — what if I use some variables interactively, and want to schedule that command to execute?

I started implementing a way to share all the environment variables, and then also all the *local* fish variables, to the server; so that it could execute the given command in the correct environment.
I'll spare you the details, but I fairly quickly realized that it's a bad road to walk.
Feels dirty, incredibly so, and is filled with caveats.
I need *another* system.

# three months

...ago, is when I started implementing this system, and writing this blog post.
The second system that I developed at the time felt *fairly* good, but I wasn't backflipping over it.
So finishing both the system and the blog post got kind of shelved.

Until a few days ago.

Niri's alt-tab / mru functionality has one downside that I think is the biggest for why I (still 😭) underuse(d :o) it. \
It shows you ***all*** the windows... What?

Well you see, I have direct hotkeys to reach almost *all* of the windows I have open at most times; so when I use alt tab to get to some window of interest, I also need to mentally filter through windows that I wouldn't be getting to with alt-tab to begin with. \
I thought “what if I could filter out some windows from alt-tab?”

And so I did! Not in a particularly sharable way, though{{fn(i=4)}}. I straight up hardcode the windows I never want to see, in a new mru scope called “Custom” — so now I have a hotkey that lets me alt-tab *only* through the windows of interest, ignoring all those I can get to directly.

Proud of my accomplishment, I thought to implement another scope for sheer fun.
One of the first ideas that came to mind was an Urgent scope{{fn(i=5)}} — that would contain only the windows that are currently urgent/require attention.
You might be familiar with the concept back on windows, where windows yelled at you covered in orange from the taskbar.

Normally an *annoying*, rather than useful feature; but a 💡 suddenly shone.
I can make terminals urgent **intentionally**.

The core problem I'm trying to design to fix, are long-running commands.
Something that I set to execute, and it maybe takes so long that I forget about it altogether.
Or worse: something that I keep checking way too often for my own sanity. Nibble 2 /ref \
I want to be *notified* of the execution finishing, and I also want to make it easy to *travel* to the window, whose command just finished.

The Urgency scope helps me solve the latter question: if a long running command makes its terminal urgent after finishing, the window will appear in alt tab, and I can __without thinking__ (very crucial) jump to it, with a simple press of a hotkey.

# complications

Solving the former question, though, is a bit more complicated.
Obviously, to be notified, I send a notification.
But I don't want to see a notification on *every* command I execute in my terminal, right?
If I already have the terminal in focus, it's pretty obvious when a command finishes.

So I need to differentiate a focused vs unfocused terminal somehow.
Fish shell has events for that :D `fish_focus_in` and `fish_focus_out` \
Yay right? Not quite... They just don't work for me for some reason?

Two functions, each one catching one of the events and doing a `notify-send` for testing did *nothing*, despite indeed loading in my configuration.
I even tried explicitly binding the escape sequence that is supposed to happen on focus in/out, to doing that `notify-send`, but that didn't work either.
It looks like foot doesn't send these events *to* fish shell? Very unfortunate.

# the sorrow /ref

The bell character is pretty cool.
Historically it was used as a sort of notification, and modern terminals allow you to configure *what* actually happens when a bell character is printed.
Crucially, it will make the window *urgent*, often even by default!

And in foot (as well as any other terminal worth using), you can configure the bell character to produce a notification as well.
The main difference being that the notification is **only** made if the window is **not** focused.

It's kind of disappointing, actually. I rely on foot to do basically all the behavior for me...
The fish events working would make my solution a lot more satisfying.

In ~/.config/foot/foot.ini:
```ini
[bell]
urgent=yes
notify=yes

[desktop-notifications]
command=notify-send -- '🚀 ${window-title}'
```

Something we lose out on here, is ability to tell *how* the command finished — with a success or a failure.
If I knew the focused status of the window from within *fish* logic, I could send a notification with ✅ on success, and with a ❌ on failure.
But what I'm forced to do instead, is just print the bell character every time a command finishes; that bell character is then “caught” by foot, and a notification is sent.

You could add a `printf \a` into your `fish_prompt`, but I do it slightly fancier and add a hook into my config.fish:
```fish
function on_fish_postexec --on-event fish_postexec
    bell
end
```

I then do something similar for nushell:
```nu
$env.config.hooks.pre_prompt = [ { printf \a } ]
```

Now *both* shells notify me when they finish running a command, *only* if their controlling terminal window is **not** in focus, *and* I don't need to execute the command in any sort of special way — I *just* press <kbd>Enter</kbd> like usual and get a notification at the end :D

There's some silver lining in just using a normal ass foot feature, though.
`${window-title}` is a cool idea!

The format I was going to do, is `❌ $cwd` / `✅ $cwd`, where `$cwd` is the basename of the current working directory of the command that finished.
But showing the *window title* is slightly better!

The window title my terminals give themselves are *already* the basename of the current working directory, unless ☝️ I gave that terminal some sort of static window title.
For instance, I have a permanently staying terminal titled `toggleterm`, that is named that way so that my hotkey to toggle it works.
And so, seeing a `🚀 toggleterm` is a lot more useful than seeing `🚀 ~`

This adventure made me more intentional about titling, altogether.
Previously, all non-statically named terminals were just called `foot`.
But to make the Custom and Urgent mru scopes more useful, showing the basename of the working directory via the title became a lot more significant.

Something helpful, as well, is that if a command *fails*, its title is now not just `niri`, but `niri ❌`.
So the issue of `🚀` being too vague is kind of healed by this.

I start a command, and go away.
After some time, it fails, and I get a notification saying `🚀 niri`.
I press <kbd>meta+r</kbd> to open the urgent window picker, and see the actual window title is `niri ❌`.
That means that I definitely should go check out what went wrong, and maybe try again.
But if all I wanted to do is to execute the command and then close the terminal (if it succeeded), here I would be able to do so, right from the mru ui, *without* needing to first actually *go* to the window — a lack of a `❌` in its title would mean that it finished successfully, and can be closed safely.

# finishing touches

Pretty goated workflow, isn't it?
I can spawn as many terminals with long-running commands as I want, and then jump to all the finished ones (or not even; just close them) very easily and **mindlessly**.
A solution you don't have to think about is often the best solution.

One small caveat though. As is, the receiver we talked about in the first section **isn't** made urgent.
I do have a separate hotkey to go to it specifically, but I love the idea of instinctively pressing <kbd>meta+r</kbd>, whenever “something finishes”, rather than having to mentally separate the receiver from other finishing terminals.

```fish
#!/usr/bin/env fish

mkfifo ~/.local/share/mine/task-scheduler 2>/dev/null
set -l this_window_id (nu --no-std-lib -n -c 'niri msg -j windows | from json | where app_id starts-with foot | where title == "receiver" | get id | first')
tail -f ~/.local/share/mine/task-scheduler | while read -z first
    and read -z second
    if test $first != ~
        set_color -o b58cc6
        string replace -r "^$HOME" '~' $first
        cd $first
    end
    while true
        set_color -o e491b2
        echo $second
        set_color normal
        set -l first_word (string split ' ' $second)[1]
        fish -c $second </dev/tty
        if test $status -eq 0
            set_color -o a9b665
            echo success
            set_color normal
            notify-send "✅ $first_word"
            echo
            break
        end
        set_color -o ea6962
        notify-send "❌ $first_word"
        niri msg action set-window-urgent --id $this_window_id
        confirm.rs failure '[r]estart' '[e]dit' '[k]ancel' | read -l response
        set_color normal
        echo
        if test $response = k
            break
        else if test $response = e
            set second (echo $second | vipe --suffix fish)
        end
    end
    cd ~
end
```

Normally, just doing `bell`{{fn(i=6)}} would do the job, but in this case it would do a bit *too* much job.
We already make notifications, and they also come with success/failure differentiation.
`bell`ing would not only make the window urgent, but send *another*, now useless notification.
So I just make the window urgent manually 🤷‍♀️

I thought about removing the distinction here altogether, just so I could do a basic `bell`, but in terms of the *receiver*, it's way too important to be able to ignore successes.
Say I schedule 5 baalorlord vods to get downloaded — I don't want to feel pressured to keep checking after *each* of those finishes downloading.

Which is unlike non-receiver command finishes — an offshoot command I may execute from a normal terminal I probably *do* want to come back to / handle as soon as it finishes, regardless of status.

# footnotes

{{hn(i=1)}} Check `man fifo 7` for more info on them.

{{hn(i=2)}} [Backup link](https://github.com/Axlefublr/wks/blob/f64cf837ee456626bcaa8d2c185b6939f5c15b5d/src/bin/confirm.rs)

{{hn(i=3)}} `moreutils` package on arch.

{{hn(i=4)}} It's available in my [niri fork](https://github.com/Axlefublr/niri), take a look if you're interested.

{{hn(i=5)}} *This* is more shareable though! [It's available as a pr](https://github.com/niri-wm/niri/pull/4208)

{{hn(i=6)}} I never made this clear, but `bell` is my fish alias for `printf \a`. Slightly cleaner semantics.
