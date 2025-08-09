+++
title = 'wayland screen recording'
date = '2025-02-16'
+++

I recently switched to wayland, and now my [ffmpeg screen recording script](https://github.com/Axlefublr/dotfiles/blob/2567bb72827619c45a4d2e554148195be6c94b74/scripts/screen-record.fish) no longer works, as it depends on x11. \
To my surprise, screen recording on wayland is *easier*!

Thanks to [wf-recorder](https://github.com/ammen99/wf-recorder), this is all you need:

```fish
wf-recorder -Dyf ~/output.mkv
```

`-y` skips the confirmation check to overwrite the file, `-D` only grabs more frames if anything changed on the screen, in theory reducing filesize. \
Pass `-a` if you wanna record audio. There are also other useful flags: one of them lets you pick a section of the screen to record, using `slurp`.

Now all you need to do to record a video, is run that command, and press `ctrl+c` on it once you finish recording. \
Through some magic I'll explain later, I have it on a hotkey as a toggle.

However! \
The #1 place I'll be sending screen recordings to is *discord*, which has a 10mb size limit without a subscription.

This is why I compress the video after, with ffmpeg:

```fish
wf-recorder -Dyf ~/i/s/original.mkv
ffmpeg -y -i ~/i/s/original.mkv -c:v libx264 -preset slow -crf 21 -b:v 2M -maxrate 3M -bufsize 4M -c:a aac -b:a 96k -movflags +faststart ~/i/s/compressed.mp4
```

This greatly reduces the size, while not being that huge of a difference in quality. Take a look!

Original, at 19mb:

{{ video(path="original") }}

Compressed, at 2.5mb:

{{ video(path="compressed") }}

You probably *can* tell a difference between the two, but I wouldn't say that it's staggering. The *size* difference **is** staggering, though! \
The compressed version, in this case, is 8 times smaller! Much easier to fit into the tight limits of discord.

Thanks to how I wrote it, the original file is kept too! \
So if I'm not restricted by size, I can choose to send the original. \
The recording is also automatically [copyl](@/uri-list/index.md)ed, letting me immediately <kbd>ctrl+v</kbd> it wherever I want! :3

{{hr(id="toggle")}}

Let's now get into how I made this into a toggle!

The fact that you can gracefully stop the recording with <kbd>ctrl+c</kbd> isn't just some hardcoded hotkey — <kbd>ctrl+c</kbd> actually sends the SIGINT signal to the running process, and it's *that* that `wf-recorder` is actually listening for.

Meaning, we can gracefully finish the current recording using:
```sh
kill -s INT wf-recorder
```

This assumes that you only have one `wf-recorder` instance running at a time, which I feel is a pretty safe assumption.

So I made myself this fish function, that I put on a hotkey in my [kanata config](@/erm/index.md):
```fish
function toggle_screen_record
    if test -f /tmp/mine/recordilock
        kill -s INT wf-recorder
    else
        screen-record.fish
    end
end
```

And then the screen-record.fish script is:
```fish
#!/usr/bin/env fish
touch /tmp/mine/recordilock
wf-recorder -Dyf ~/iwm/sco/original.mp4
ffmpeg -y -i ~/iwm/sco/original.mkv -c:v libx264 -preset slow -crf 21 -b:v 2M -maxrate 3M -bufsize 4M -c:a aac -b:a 96k -movflags +faststart ~/iwm/sco/compressed.mp4
rm -f /tmp/mine/recordilock
sl.fish ~/iwm/sco/compressed.mp4
```

So, to go over the whole process, from a neutral state: \
I want to start a recording, so I press my hotkey. \
The `/tmp/mine/recordilock` lockfile doesn't exist, so we go into the second branch, starting the screen-record.fish script. \
The lockfile is created — the next time we press the hotkey, we'll go into the first branch instead of the second one. \
We start recording, and waiting for SIGINT to be sent to `wf-recorder`.

I finish my recording, so I press the same hotkey again to complete it. \
This time, since the `/tmp/mine/recordilock` file exists, we go into the first branch — it sends the SIGINT signal that `wf-recorder` is waiting for.

Now that the `wf-recorder` process is finished (which takes some time), the initial script can continue execution. \
It does the compressing with ffmpeg, then deletes the lockfile. We could, if we wanted to, immediately start a new recording now. \
The `copyl` that is now called `sl.fish` because I'm returning half a year after initially writing this blog post{{fn(i=1)}} copies the uri link to the recording into my clipboard, letting me easily paste it into anywhere.

This is a lot better!
Than dedicating a terminal window for the recording script, and then manually pressing <kbd>ctrl+c</kbd> on it to finish it. \
Doing so is not too bad all things considered, but is a bit too close to the ~~IBS~~ OBS workflow.

So now we *don't* see when the recording starts and ends 🥳... Wait, what? \
Right! Let's integrate this recording toggle into waybar, so it's actually clear to us when we're recording, when we're compressing, and when we're done.

# waybar

Are you familiar with named pipes? No? \
They're really convenient for making waybar widgets the text of which you need to modify externally. \
I'm not gonna explain them in depth, but the relevant manpages are the following: `mkfifo.1`, `fifo.7`.

Crucially, FIFOs don't operate via writes to disk, despite having filepaths, and are instead in-memory. \
That makes them quite a bit faster and a lesser hog of our system resources!

If we use a normal file, we need (should?) to create some sort of logic to intermittently clear up old data in the file. \
Or I guess would could blammo it into `/tmp` and hope that we will always be rebooting before the bloat ever becomes a problem. \
FIFOs avoid this issue with the semantic that the data is read-once — one side is expected to continuously consume the content, and once it has read something, the data is no longer stored in the fifo. \
There can be multiple writers, though — so you don't need to create some convoluted writer service or anything 😌

Let's create the fifa with ↓
```fish
mkfifo ~/.local/share/mine/waybar-screen-record
```
Conveniently, once you create a fifo, it stays forever — it's not some temporary file you have to set up on every boot or anything. \
You can of course later delete it like a normal file.

Now we create a custom module in waybar ↓
```json
"custom/screen-record": {
  "exec": "/home/axlefublr/fes/dot/waybar/screen-record.fish",
  "format": "{}",
  "hide-empty-text": true
},
```

We're using the output of a script as the text of our module/widget. \
Every new line that is outputted by the script, *replaces* the text of the widget; a new *blank* line resets it.

The script is just:
```fish
tail -f ~/.local/share/mine/waybar-screen-record
```

Now all we have to do to update the widget, is to *somehow* write a line into the `~/.local/share/mine/waybar-screen-record` “file”. \
It's actually not a file 🤓☝️ \
And yet, we interact with it exactly how we interact with other files: a simple ↓
```fish
echo something >~/.local/share/mine/waybar-screen-record
```
Will make our widget display “something”. And ↓
```fish
echo >~/.local/share/mine/waybar-screen-record
```
Will clear it.

Now let's come back to the toggle function and recording script, to make them interact with our new waybar module :3

```fish
function toggle_screen_record
    if test -f /tmp/mine/recordilock
        echo killing >~/.local/share/mine/waybar-screen-record
        kill -s INT wf-recorder
    else
        echo starting >~/.local/share/mine/waybar-screen-record
        screen-record.fish
    end
end
```

In the toggle function, we report sending the kill signal / starting the recording script. \
By all accounts, the next step after that happens so fast that we are probably never going to see `killing` / `starting` in our waybar module, but if anything ever holds up suspiciously long, this can be helpful for debugging.

```fish
touch /tmp/mine/recordilock
echo recording >~/.local/share/mine/waybar-screen-record
wf-recorder -Dyf ~/iwm/sco/original.mp4
echo compressing >~/.local/share/mine/waybar-screen-record
ffmpeg_compress ~/iwm/sco/original.mp4 ~/iwm/sco/compressed.mp4 -preset slow -crf 21 -b:v 2M -maxrate 3M -bufsize 4M -b:a 96k
rm -f /tmp/mine/recordilock
echo copying >~/.local/share/mine/waybar-screen-record
sl.fish ~/iwm/sco/compressed.mp4
echo >~/.local/share/mine/waybar-screen-record
```

Now it's clear what's currently happening, if anything! \
After you press the hotkey the second time, you can quickly confirm that the recording did indeed complete, and is now compressing. \
Once it's in the compressing stage, you basically wait for the waybar module to become empty again — *that's* the marker that it `copyl`ed, and is now pasteable 🚀

{{video(path="recording")}}

I can't exactly show you all the steps, but here's my waybar saying “recording”, lol!

# footnotes

{{hn(i=1)}} This whole section of me explaining how to make a toggle hotkey for this is new! \
Writing this on 2025.08.04 \
You see, previously (as can be seen in the example recordings above) I had a wack solution that used kitty's `send-signal` capabilities, that was too hyperspecific to share.
