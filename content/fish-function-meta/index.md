+++
title = 'the fish function meta'
date = '2026-06-20'
draft = true
+++

In other shells, when you want to add a custom function, you add it to your config. Obviously. \
Maybe you put your functions in a separate file and then source it, but same difference. \
Bash, zsh; even nushell works this way.

Of course, instantiating functions takes some startup time off of you.
It's silly to put every niche functionality you wrote, as a *function* into your config — better to create a separate script instead.

So I don't blame you for continuing this trend after switching to fish shell.
But the thing is, if used correctly, functions are COMPLETELY FREE in fish shell!

{{hr(id="it-is-sad-how-wrong-you-are")}}

Fish has a custom lazy-loading mechanism for functions. \
When you try to execute a command, fish (roughly) starts with these steps:
1. Is the command a fish function currently loaded in this session? → use it
2. Is the command, if we appended a `.fish` extension to it, a file in `~/.config/fish/functions/`? → load it and use it

So by putting a function directly in your config, you are *always* force loading it.
Force loading it on *startup*, before your prompt is drawn. \
There are very very few functions that need to be available before your prompt, and even for those there is actually no benefit to *not* lazyload them.

So instead of putting the following ↓
```fish
function bell
   printf \a
end
```
into your config.fish, put it into `~/.config/fish/functions/bell.fish`.
Now it won't take ANY of your startup time, yet becomes available when you first call it; with no extra shenanigans on your part, with regards to how you call it.

End of blog post, right? Well not quite. \
While this *is* the entirety of the mechanics, I concede that it's a bit inconvenient to do manually. \
But thankfully, so does fish shell!

# built in ux

Fish shell (like every other shell) uses the exact same language in scripts, as it does interactively.
The…
```fish
function bell
   printf \a
end
```
… that you put into your config.fish, you could also paste in and execute *interactively* in a shell session, and that function will become available for that session.

It won't become available in other sessions though, and will dissapear once you close the current session. \
We can fix that, though!
```fish
funcsave bell
```
is what you can execute to place the `bell` function we interactively defined earlier, into its own file, at `~/.config/fish/functions/bell.fish`.

Now *not only* do we have this function available in the current session, but we'll also still have it in *future* sessions, and EVEN in all *current* **other** sessions{{fn(i=1)}}.
As far as I know, fish doesn't define a specific timeframe of when the new function gets synced to *other* sessions, but I would roughly describe it as “in due time”.

Well, if you just *created* the `~/.config/fish/functions/bell.fish` file by `funcsave`ing `bell`, it will of course appear instantly in other sessions.
But if you *overrode* the already existing file, the update will arrive to other sessions in a few seconds, in my experience.

{{hr(id="lowdiff")}}

This is *already* better than putting the function into your config.fish.
If you have multiple terminals open, you don't gotta do shit to update them — by the time you focus them again, they are probably already updated. \
Something fun I've experienced many times is after editing my `fish_prompt`, I press <kbd>Ctrl+l</kbd> (in a different terminal) to redraw the screen, and instantly see the updated prompt.

However. Personally, I don't find it comfortable to write fish functions interactively; I'd much prefer to write them in my editor.
Yeah I'm losing tab-complete and history suggestions by doing that, but I'm [not a big fan](https://axlefublr.github.io/harp/) of tab complete to begin with.

Fish comes with two built in hotkeys that let you edit the current commandline in your `$EDITOR`: <kbd>Alt+e</kbd> and <kbd>Alt+v</kbd>.
They are alternatives to each other, no difference in behavior{{fn(i=2)}}.

{{video(path="edit-commandline")}}

Using these hotkeys, I can write a new fish function in my editor! \
Buuuuut, I have this [fancy snippet](@/harp/index.md) for fish function boilerplate, and you don't.
Writing out `function` + `end` is still not peak ergonomics.

No need for anything fancy though! Try `funced`:

{{video(path="funced")}}

`funced` stands for func*edit*.
If you `funced` a function that doesn't exist yet, you get the `function` + `end` boilerplate for free.
But if you `funced` a function that *does* exist, you **edit** its code in your editor!

Notably you do still need to `funcsave` afterwards.
Or use `funced -s` to do it in one step!

# aliases

Unlike in bash/zsh, fish aliases are actually *just* a shorthand for creating a function.
The following two are EXACTLY equivalent:
```fish
alias rmi 'rm -i'
```
```fish
function rmi --wraps rm --description 'alias rmi=rm -i'
   rm -i $argv
end
```

And since aliases are just a syntax sugar for a function, you can `funcsave rmi`, just like you could a normal function!
Except aliases provide a nifty flag that lets you both create the function and `funcsave` it in one step:
```fish
alias --save rmi 'rm -i'
```
Will put the following:
```fish
function rmi --wraps rm --description 'alias rmi=rm -i'
   rm -i $argv
end
```
Into `~/.config/fish/functions/rmi.fish`

The contents of the `--description` flag appears when you tab complete.
`--wraps` inherits the completions of the specified command; in this case the completions of `rm`.

# cleanup

If you want to *delete* a function you've `funcsave`d in the past, you don't have to go manually track down its file.
You can use `functions -e testie` followed by `funcsave testie`. \
The first command removes it *from your session* (and prevents the current session from lazy-loading it again), and the second command deletes `~/.config/fish/functions/testie.fish`.

Pretty annoyingly, `functions` *doesn't* provide a nifty flag for you to use to `funcsave` at the same time.
But you can easily create your own wrapper:
```fish
function fumolish
   functions -e $argv
   for fun_name in $argv
      funcsave $fun_name
   end
end
funcsave fumolish
```

`functions -e` accepts specifying a list of functions at once, but `funcsave` does not, so we blammo in a loop.
So now you can do `fumolish testie` to remove it both from your session, and all future ones.

I'm explaining this mostly only for completeness, though.
There's not much reason to clean up your functions considering they are COMPLETELY FREE in startup time; so I never bother to.

# elephant

There is still one, but *large* benefit of blammoing a function into your config.fish. Organization.

Supposedly, in the `funcsave` workflow, you make your function and continue to never come across it ever again.
I mean, in what world would you actively check the files in `~/.config/fish/functions/`, right? \
If you never see your functions *naturally* as you're configuring parts of your system, it can be easy to forget that some of them even *exist*.
You might accidentally recreate a function that you already have just because you forgot about it.
Or maybe, you *do* remember that it exists, but can't quite recall the name. \
And tbh, grepping my dotfiles repo I find much more natural than `cd`ing into `~/.config/fish/functions/` and looking there.
Despite the latter being technically more efficient.

Even still, it's not like the functions are organized there at all.
In a config.fish workflow, you might put some functions in one place; others in another.
Group them up by usecase, or any arbitrary category for that matter; even if you don't remember the name of some function, you'll know the category it belongs to, and jump there to edit it.

With multiple functions stored in a single file, it also becomes a lot easier to *edit* a bunch of functions at a single time.
Rather than entering a whole ass file just to edit a *single* alias. \
I mean hell, even if I wanted to do that, I don't wanna go execute `funced -s` in some other terminal — I want to use the editor that I *already* have open in my dotfiles repo!
And I **refuse** to bloat it with one billion separate files to achieve that.

It is **these** elephants that the title of this blog post represents addressing. \
The fish function meta™ is my own approach to fish function management, which lets me retain the organization of a config.fish-like workflow, yet get the massive startup speed benefits of `funcsave`. \
No, don't put `funcsave` into your config.fish, that's the worst you can possibly do /silly /but-true

# the fish function meta

The concept is pretty simple.
Instead of putting your functions into config.fish, put them in a separate script. Put a relevant `funcsave` after every function.
To update the functions stored in `~/.config/fish/functions/` to the ones defined in this script, after you add a new function or edit an existing one, simply run the script.

Take a look at my [core.fish](https://github.com/Axlefublr/dotfiles/blob/main/fish/fun/core.fish) for example.
```fish
function arebesties -a fileone filetwo
    test (stat -c %i $fileone) -eq (stat -c %i $filetwo)
end
funcsave arebesties >/dev/null

alias --save bell 'printf \a' >/dev/null

function catait
    inotifywait -e close_write -e close_nowrite $argv &>/dev/null
    cat $argv
end
funcsave catait >/dev/null

alias --save dedup 'awk \'!seen[$0]++\'' >/dev/null

function dof
    diff -u $argv | diff-so-fancy
end
funcsave dof >/dev/null

function dot
    echo dot >~/.local/share/mine/waybar-red-dot
end
funcsave dot >/dev/null
```

In it, I store functions that I consider *core* to my fish shell experience.
Extensions of the standard library, almost.

If I wanted to edit the behavior of `catait`, for example, I would edit it here and then run the script, updating the *stored* `catait` in the process. \
I like to keep the functions sorted alphabetically for easier organization, but that's optional.

`funcsave` makes a status message saying “your function x got saved to ~/.config/fish/functions/x.fish” which is fine interactively, but annoying when using this workflow.
I run the script *from my editor*, so I see a comically long output of `funcsave`s when I do. \
That's why I put `>/dev/null` after every `funcsave`: to silence that message.
The `--save` of `alias` is effectively a `funcsave` too; so I silence it there as well.

In my [dotfiles repo](https://github.com/Axlefublr/dotfiles), I make a [subdirectory called fun/](https://github.com/Axlefublr/dotfiles/tree/main/fish/fun), where I store all such function-defining scripts.
I add it to `$PATH` by running `fish_add_path ~/fes/dot/fish/fun`, so that every function-making script I can execute in my terminal with *just* its name, without needing to point to its full path.

❗path caveat should be a footnote

Using separate scripts, I get to define the categories that I split my functions to, basically.
Although just using one ginormous function-defining script is honestly viable too.

I use builtin.fish for builtin fish functions, such as `fish_greeting`, `fish_title`, etc. \
Due to how huge it is, prompt.fish is a separate file for my custom fish shell prompt. \
bindings.fish is where I configure my interactive shell bindings, as even *those* you shouldn't put directly into your config.fish. \
ffmpeg.fish is my collection of interactive video and audio editing functions, using `ffmpeg`. \
magazine.fish is the implementation of my [magazine system](@/magazines.md).

❗I was optimizing fish startup time, and you'd think that all the billion functions I make would catch up to me, but no they are genuinely free. I have 201 functions currently, for context
❗why function over script? if this is something that I *may* call from fish shell, there's not much reason not to. starting a separate script file takes some time, just like any other binary / executable; but running a function doesn't have the cruft of all that, so it's a lot instanter. I might as well do `fish -c do_super_niche_action` in various places over `do_super_niche_action.fish`, for that nifty speed benefit. gets even insaner with `fish -Nc`, but unlike with nushell, I haven't been needing *that* level of optimization

❗is this all in the docs? *yeah*, but it's pretty rare for someone to read the docs on the shell they just started using. they just use it and bob's their uncle. and even if they do, probably not the *entire* docs to discover that this is THE fish way; as imo fish doesn't make this entire shenanigan clear enough in the mainline documentation

❗lazyloading completions

# footnotes

{{hn(i=1)}} What I'm calling a “session” is after you execute `fish` to enter fish, but before you exit it with `exit` or <kbd>Ctrl+d</kbd> or closing the terminal.
Executing an inline fish script via `fish -c '…'` is also a session; until the command finishes executing.
Running a fish script counts as a session as well, although it is *not* an **interactive** session.

I'm more so explaining how *I'm* using the term in this blog post, rather than how fish shell itself uses it, though.

{{hn(i=2)}} Slightly fun fact: <kbd>Alt+e</kbd> is mnemonic for $**E**DITOR, and <kbd>Alt+v</kbd> is mnemonic for $**V**ISUAL.
Both of these environment variables are meant to contain your favorite text editor, but EDITOR is more recent than VISUAL.

❗some readers may encounter [this *exact* error message] when executing this. in such cases, it's entirely their fault
