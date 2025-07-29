+++
title = 'blammo'
date = '2025-07-28'
draft = true
+++

I use the [yazi](https://github.com/sxyazi/yazi) file manager, and like it a lot! \
Most of my file operations, including even `cd`ing sometimes, is done directly from yazi. \
Which is why I add a couple of powerful special hotkeys to it!

For example, I quite often need to cut videos; \
Unlike a normal person, I refuse to use an actual editing app, and instead I do it with `ffmpeg`. \
I wrote some [interactive ffmpeg fish functions](https://github.com/Axlefublr/dotfiles/blob/main/fish/fun/ffmpeg.fish), and integrated them into yazi: \
On a press of a hotkey, all the files that I selected in yazi get passed to the function, and iterated on one by one.
I then answer “questions” to edit the videos however I wish.

This is really neat, but not super scaleable in a sense: \
I can't make a hotkey for every ad-hoc action, can I?

It makes sense to have shortcuts to the really common actions, but you won't know if an action is common or not until you've already done it quite a few times. \
If you're too quick, you might end up with a “dead” hotkey in your configuration, \
But if you don't provide yourself *any* shortcut, doing The™ Action™ might be really laborious.{{fn(i=1)}}

I came up with an idea that lets me seamlessly connect yazi's selected files to my shell, and I find the design really cool and nushell-like :D

# fishangling

Let's start with how I reached the idea in the first place; why it even needed to exist.

Yazi allows you to make mappings that call shell commands. \
In those, you have access to `$@`, which means “absolute filespaths to all the currently selected files”. \
To handle spaces, you seem to need to specifically enclose the `$@` in doublequotes.
```
'shell --confirm -- ouch decompress "$@"'
```
The `--` argument allows us to not have to quote the command itself — everything after `--` is considered a raw part of the actual command.

This works, as you'd expect! \
I can select multiple archives and decompress them all with a hotkey{{fn(i=2)}}. \
But it's rare for me to just call commands directly; \
Often times, I write some sort of functionality in a fish shell function, and invoke it with `fish -c`. \
All my various ffmpeg functions are written in fish, for example:
```
'''
shell --block --confirm -- fish -c 'ffmpeg_convert_to_mp3 "$@"'
'''
```
Except... this doesn't work. \
`"$@"`, that *just* expanded absolutely correctly, no longer does inside of an argument to `fish -c`.

You see, I immediately assumed that the `$@` syntax in yazi is its *own* thing, rather than the usual POSIX `$@`; \
Fish shell actually uses `$argv` to mean the same thing, so I guess I can simply use that, neat :D
```
'''
shell --block --confirm -- fish -c 'ffmpeg_convert_to_mp3 "$argv"'
'''
```
Except...... It also doesn't work. \
So `$@` *is* a yazi-specific thing, and perhaps it only expands correctly when provided as a separate argument, on its own, rather than part of a bigger argument 🧐

I'm being pretty vague here, with “doesn't work”, but that's because the semantics I still don't fully understand, and my experiences with them are very blurry. \
I feel that sometimes, it expanded all the paths into a single string, rather than multiple arguments, \
other times, didn't expand anything at all, \
maybe it didn't handle spaces correctly?

Regardless, I decided to create my own solution for providing the list of arguments into fish from yazi, that worked by far more clear and guessable rules.

Welcome to “blammo”! \
Yeah, I really didn't think much about the name of this thing, and it just kinda stayed “blammo” forever 😭 \
If you've been reading my blog posts for some time, you might've noticed how much I love this word, lol. \
Fun fact, my insane overuse of it comes directly after [this video](https://www.youtube.com/watch?v=Eqo7rMUKm9A)!

# blammation

I wrote this simple yazi plugin:

```lua
--- @sync entry
return {
	entry = function()
		local collector = ''
		for _, url in pairs(cx.active.selected) do
			collector = collector .. tostring(url) .. '\n'
		end
		if #collector == 0 then
			collector = tostring(cx.active.current.hovered.url) .. '\n'
		end
		local file = io.open('/home/axlefublr/.cache/mine/blammo', 'w')
		if file then
			file:write(collector)
			file:close()
		end
	end,
}
```

Yazi is in very active development, for more better than for the worse; \
So don't expect this to straight up work if you're reading this post in the future.

The main idea, though, is pretty simple: \
We take the full paths of all files we have selected, and convert them into a string which has one path per line. \
If we don't have any *selected*, we simply take the filepath that our cursor is currently on. \
And we write that to some file.

The path doesn't matter, but I like to use `~/.cache/mine` to hold various cache-like files that I don't want removed on reboots, which would happen if I put it in `/tmp`. \
I have the opposite idea also, where I hold files I *do* want eventually auto-cleared in `/tmp/mine`.

Anyway, after you put the above code into `~/.config/yazi/plugins/blammo.yazi/main.lua`, you can now access it in your yazi mappings with `plugin blammo`. \
When you call the plugin, it stamps the current selected files into a file, which you can immediately read as a list of arguments. \
In shells, you can `cat some-file`, and it will convert each line in a separate argument. \
Which is very clean, simple, safe, and nifty!

We can now fix the previous ffmpeg mapping I was trying to create:
```
[ 'plugin blammo', 'shell --block --confirm -- fish -c "ffmpeg_convert_video (cat ~/.cache/mine/blammo)"' ]
```
And this **does** work 🥳

Having to type out the blammo filepath for every hotkey is a bit laborious, though, so I made myself a nifty little fish alias:
```fish
function blammo
    cat ~/.cache/mine/blammo
end
```
So now, the yazi mapping is:
```
[ 'plugin blammo', 'shell --block --confirm -- fish -c "ffmpeg_convert_video (blammo)"' ]
```

This solves the issue I had at the time, but highlights something interesting: \
I was just able to, effectively, reach into the context of yazi, from outside of yazi 🤔 \
As long as it's properly updated, nothing is stopping me from using `(blammo)` just interactively in my shell!

I made my quit and [suspend](@/suspend/index.md) hotkeys `blammo` first, so that leaving yazi ensures the blammations are up to date.
```
{ on = ':', run = [ 'plugin blammo', 'quit' ], desc = 'Exit the process' },
{ on = '<C-z>', run = [ 'plugin blammo', 'suspend' ], desc = 'Suspend the process' },
```
Now I can select some files in yazi, suspend with <kbd>ctrl+z</kbd>, and execute some arbitrary shell command on the files I had selected!

I recently had a usecase, where I wanted to convert a video into webp (which you can apparently do):
```fish
magick (blammo) (blammo | path change-extension webp)
```

I love it!! \
But it's a *biiiiit* wordy — typing out `(blammo)` is more syntax than I'd like 🤔

So I came up with a funny but reasonably solid hack! \
At the end of my `fish_prompt` function, I put the following line:
```fish
set -g in (blammo 2>/dev/null)
```

`fish_prompt` is the function that runs every time your prompt needs to be drawn. \
Meaning, *before every command*. \
So, we put yazi's latest selected files into a global variable `in`, that we can then use directly as the list of filepaths!
```fish
magick $in (path change-extension webp $in)
```
Semantically, this is quite a bit better :D

In nushell, `$in` means “whatever was piped into the current thing”, so here it's as if we're piping the list of filenames into fish, quite neat!

# footnotes

{{hn(i=1)}} Sounds similar to the types of situations that [harp](@/harp/index.md) solves, doesn't it? \
You're correct! Harp could be implemented into yazi to provide a pretty nice productivity boost, but *so far* I don't find myselfexecuting enough commands from the context of yazi to really need harp.

{{hn(i=2)}} I'm aware of the built in yazi unarchiver, but it's been super wonky for me. \
I'd much rather use the behavior I'm more familiar with, using `ouch`. \
This hotkey still exists, but I actually made “opening” archives automatically use `ouch decompress`. \
How did I do that? Exercise for the reader 😌
