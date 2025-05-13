+++
title = 'autocurling'
date = '2025-05-13'
+++

Often times, various programs don't provide you a way to look at the default config file locally, but do provide one in the github repo. \
Having to 1. need internet and 2. use my browser to get to some default config is quite meh. The documentation being the readme falls into a similar issue. \
Cloning the repo so that you have it locally is overkill, too.

Let me explain the idea of how I solve this problem.

You can `curl` files directly from github{{fn(i=1)}} by converting a normal link to a file into its raw version:
```
https://github.com/Axlefublr/dotfiles/blob/main/kanata/kanata.kbd
```
…Turns into…
```
https://raw.githubusercontent.com/Axlefublr/dotfiles/refs/heads/main/kanata/kanata.kbd
```
So, you replace `github.com` with `raw.githubusercontent.com`, and replace `blob` with `refs/heads`. \
I wrote a fish function that takes the link from stdin, and converts it for me:
```fish
function lh
    read | string replace 'github.com' 'raw.githubusercontent.com' | string replace blob refs/heads
end
```
Which is useful because in helix, I can pipe my current selection(s) into a shell command, and replace them with the output, letting me transform github links I copied pretty painlessly!

So, once we get the raw link, we can `curl <the-link-here> -o output-file` to grab a remote github file into a local one. \
This is pretty awesome! But just grabbing the file once is not enough — it can become outdated.

My idea is to store all the links I want to keep locally, so that they can be updated daily automatically. \
Using a [magazine](@/magazines.md) is awesome for this usecase, as it's both easily accessible and modifiable — adding new links isn't going to be bothersome.

My magazine C now contains this:
```
defconf/waybar.jsonc https://raw.githubusercontent.com/Alexays/Waybar/refs/heads/master/resources/config.jsonc
defconf/niri.kdl https://raw.githubusercontent.com/YaLTeR/niri/refs/heads/main/resources/default-config.kdl
defconf/dunst.ini https://raw.githubusercontent.com/dunst-project/dunst/refs/heads/master/dunstrc
defconf/ov.yaml https://raw.githubusercontent.com/noborus/ov/refs/heads/master/ov.yaml
source/systemd-exponential-restart.c https://raw.githubusercontent.com/systemd/systemd/refs/heads/main/src/core/service.c
```
First, I specify a path to where the file should be downloaded; then the actual link. \
The path is relative to `~/.local/share/frizz` — a directory I created specifically for files that will be downloaded like this. \
I then have a fish abbreviation (similar to an alias) that `cd`s me to that directory and does `helix .` — I can quickly open a new terminal and instantly fuzzy search through my downloaded files this way.{{fn(i=2)}}
```fish
function frizz
    for curlie in (tac ~/.local/share/magazine/C)
        set -l bits (string split ' ' $curlie)
        curl $bits[2] --create-dirs -o ~/.local/share/frizz/"$bits[1]"
    end
    notify-send frizz
end
```
This is the function that actually does the downloading, by reading the magazine in *reverse order*. \
The reason to download specifically in reverse order, is that aside from being executed daily, this function is also executed every time magazine C *changes*.

I add some new link to magazine C, and that new link gets download priority, letting me start doing things with the file quicker, while all the other links are also redownloaded just to be safe.

You can achieve this functionality just by using `inotifywait` directly, but I find a systemd path unit to feel safer / more stable. \
Put the following in `~/.config/systemd/user/frizz.path`:
```ini
[Path]
PathModified=%h/.local/share/magazine/C

[Install]
WantedBy=paths.target
```
And this in `~/.config/systemd/user/frizz.service`:
```ini
[Unit]
StartLimitBurst=0
StartLimitIntervalSec=0

[Service]
Type=oneshot
ExecStart=env fish -c frizz
```
Then execute `systemctl --user enable --now frizz.path`, and you should be good to go.

{{hr(id="done")}}

What I like the most about my approach, is that rather than hyperfocusing on raw github links, it simply wants a "link that will be `curl`ed". What exactly you get from it as a result is a completely different question — basically it's quite satisfyingly flexible. \
Think of the many programs you've seen whose installer is some form of `curl link | sh`. You could now instead add that link to magazine C and soon open it in your editor, to confirm the safety of; *Then* you can manually execute it. As a bonus, you get to autoupdate that installation script, if that's for some reason something you want to do. \
You can curl straight up files this way, too. If there's some place that you can curl to download some file (that may update over time), that's another usecase for frizz. \
Truthfully I don't know the extent of this idea, but it sure feels quite powerful!

# footnotes

{{hn(i=1)}} As well as other git remotes I'm sure, I just don't know how.

{{hn(i=2)}} This was truthful at the time I wrote it, but at this point it's a lie :p \
Now I use a harp{{fn(i=3)}} to get to the frizz data directory, and I have an abbreviation `e.` that resolves to `helix .`, which is somewhat hilariously significantly nicer than having to `e .`, which was the case before. Crazy how much of a difference really small things can make! (that's what she said)

{{hn(i=3)}} A "mostly travelling"-related concept I came up with and have been dragging along through programs I use a lot. The actual program behind this is [here](https://github.com/Axlefublr/harp), but for an actual introduction about what harps can *do* for you, read the harp section in my [helix fork](https://github.com/Axlefublr/helix). (TLDR is that you can interactively but permanently alias files and directories, for fastest access time)
