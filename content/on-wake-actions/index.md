+++
title = 'change wallpaper on wake'
date = '2025-04-30'
+++

I managed to do something really neat.

I wake up at almost random times, and so simply changing my wallpaper every day at 5am will be wonky. \
Sure, often times that does match up to be before I wake up, but other times it's in the middle of the day for me.

# change wallpaper

First, we start by having a script that will actually do the wallpaper change.

```fish
swww img -t any --transition-fps 255 --transition-duration 3 (propose.rs wallpaper 70% ~/.local/share/magazine/T)
```

I'm sure you already know what it is for you, assuming that you already set up a wallpaper daemon. \
If you haven't, I strongly believe `swww` is just directly the best one (for wayland).

"Wtf is [propose.rs](https://github.com/Axlefublr/dotfiles/blob/main/eli/propose.rs)" you might ask? Ah! It is proof that [my idea](@/randomization-sucks/index.md) ended up being not that viable, and so I changed my approach. Irrelevant for this blog post though.

# catch suspend

The next step is to make the suspend handler. \
I assumed at first I would be able to simply use systemd's existing mechanisms to make my script run `After=` suspend.target, but the docs specifically mention how that is not a thing.

Intead, you put all the scripts you want to run on suspend into `/usr/lib/systemd/system-sleep/`.

Two caveats with this! The scripts run twice: before *and* after suspend. \
To differentiate, they are passed either `pre` or `post` as the first argument, among other things you can learn about by reading `man systemd-sleep`.

The more annoying to deal with part, is that these scripts are explicitly stated in the docs to not interact with the currently active user graphical session. In simpler words, these scripts should assume that all of your wm, waybar, and significantly *wallpaper* daemon aren't a thing and won't work, at the time of execution. \
Notably, this is not an issue of running on root level (which it does), that could be solved by the `-u` flag of `sudo`; This is simply a timing issue.

This is why we can't just make *this* script run the wallpaper changing script — it won't work. \
Instead, we will do it in a very roundabout way.

```fish
#!/usr/bin/env fish

if test $argv[1] = post
    touch /tmp/cami-just-suspended
    chown axlefublr:axlefublr /tmp/cami-just-suspended
end
```

When the computer wakes up (`post`), we will creat a file that is both accessible by root and the user. I `chown` it for good measure, but I'm not sure it's necessary.

Next, we create a *user*-level systemd path unit. A path unit is simply a thingy that watches for filechanges. You could alternatively simply have a script that uses `inotifywait`, but I find a systemd unit to be more trustworthy.

Place this file as `~/.config/systemd/user/wake.path`

```ini
[Path]
PathExists=/tmp/cami-just-suspended

[Install]
WantedBy=paths.target
```

And this file as `~/.config/systemd/user/wake.service`

```ini
[Unit]
Requisite=graphical-session.target
After=graphical-session.target

[Service]
Type=oneshot
ExecStart=rm /tmp/cami-just-suspended
ExecStart=/home/username/r/dot/scripts/wallpaper-change-script.fish
```

To enable this, you'd run `systemctl --user enable --now wake.path`. Thanks to the `--now` flag, it will activate immediately. \
Thanks to the `WantedBy` in the `[Install]` section of the path unit configuration, this will autostart on your next boots 👢

Because I'm not sure how soon the suspend handling scripts come into effect, I'd reboot for good measure. \
But in case you don't need to, test it out!

Sleep / suspend the computer, then wake it up. The wallpaper should change! :D

If you set this all up, and `systemctl --user status wake.service` shows an error saying "wallpaper-change-script.fish wasn't found", it is entirely your fault.¹ /j

{{ hr(id="ps") }}

¹ I'm making fun of the strawman reader that didn't intuit to change `/home/username/r/dot/scripts/wallpaper-change-script.fish` to the actual location of their wallpaper changing script. \
In the case you forgot to chmod the script (or the suspend handling one), that's far more understandable.
