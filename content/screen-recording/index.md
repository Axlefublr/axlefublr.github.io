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
Through some magic I won't bother to explain, I have it on a hotkey as a toggle.

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
