+++
title = 'systemd timers'
date = '2025-07-01'
draft = true
+++

I've been using systemd timers for a while, but I don't particularly have a resource to point people to when talking about it. \
`man systemd.timer` is *a* resouce, but not one that would get you up and running quick, if you don't know about all the extra context from the various other systemd manpages. \
I hope to make this blog a good place to get started with using systemd timers; \
Later on you will be able to expand your knowledge by reading the manpages.

You might have heard of `cron`. It's a thingy that lets you run some script every x amount of time, or on every xx:xx:xx time.

Let's say you want to run a system update script daily. \
You wake up no later than 10am, so you make the script run every day at 10am. \
Until you do wake up later, and notice that your script didn't run.

That's where the power of systemd timers comes into play{{fn(i=1)}}. \
One of the capabilities of systemd timers is that they *can* (not required to) run even if the timestamp was “skipped” (by your computer not being awake at the time). \
But let's start with the basics first.

Let's create the configuration for a daily update script. \
First, you need to create a `.timer` file. \
Which you can do interactively:

```sh
systemctl --user edit --full --force daily.timer
```

That's for if you're a psycho that `git`s the entire `.config` directory, or even worse — doesn't back up their dotfiles at all. \
I'm going to show you a more practical way of creating this file in a more easily backuppable way. \
For now, simply create it wherever would make sense for you. \
For me, it's in `~/fes/dot/systemd/daily.timer` — in my dotfiles directory/repository. \
We'll go through the meat and potatos and then come back to this question of how to make it work.

```ini
[Timer]
OnCalendar=*-*-* 05:00:00
Persistent=true

[Install]
WantedBy=timers.target
```

`Persistent=true` is what makes the timer execute even if the timestamp already elapsed. \
But only after you actually enable the timer.

My sleep schedule is really unstable, so there's no one time throughout the day when I can be sure my pc will be awake. \
It running “as soon as it can” rather than *specifically* at 5 am is very helpful in that sense.

The `[Install]` section lets the timer automatically start on boot. \
‘Enabling’ and ‘starting’ are separate concepts in systemd. \
When you start a unit, that means it will now start being active. \
When you enable a unit, that means that it will enable *hooks* that will start this unit.

So if we were to enable this timer, we would mean “this timer will start being active on boot” — but it won't start being active *right now*. \
Alternatively, we could *start* the timer, and it would be active until we shut down. After a reboot, we would need to start the timer again. \
Usually, you both enable *and* start units.

The `OnCalendar` line is what you will be realistically changing; everything else in the example file should probably stay the same. \
`OnCalendar` lets you define the *when* of the timer.

Run `man systemd.time`, search for `CALENDAR EVENTS`, and scroll for a bit. You'll see a nice table of examples that will probably show to you how to achieve the specific “when” that you want for your timer.

Ngl, so far I've simply been asking chatgpt to make the timestamp for me, because it's easy to check if it's correct by using `systemd-analyze calendar`:

```
~ 󱕅 systemd-analyze calendar '*-*-* 05:00:00'
Normalized form: *-*-* 05:00:00
    Next elapse: Wed 2025-07-02 05:00:00 +08
       (in UTC): Tue 2025-07-01 21:00:00 UTC
       From now: 23h left
```

There is another basic “when” setter aside from `OnCalendar` — `OnStartupSec`. \
It lets you run your script on *login*, after n seconds. \
Despite it ending in `Sec`, it can actually also take in a duration specified otherwise, like `2min 3sec`. \
For how to specify a duration in systemd, check `man systemd.time` and search for `PARSING_TIME_SPANS`. \
If you don't wanna bother, you can just take out a calculator to figure out the amount of seconds that your timespan is (which may end up being inaccurate, due to how dates work overall).

This will make the update script run every day at 5 am, but also 60 seconds after you log in:

```ini
OnCalendar=*-*-* 05:00:00
OnStartupSec=60
```

As you can see, you can use one or the other or both at the same time. You can also have *multiple* of either of them, in a single config:

```ini
OnCalendar=*-*-* 05:00:00
OnCalendar=*-*-* 12:00:00
OnCalendar=*-*-* 17:00:00
OnCalendar=*-*-* 00:00:00
OnStartupSec=60
OnStartupSec=30min 50sec
```

Personally, I don't recommend using a systemd timer with `OnStartupSec`. It's not inherently faulty, but it may *surprise you*. \
Generally, your wm / de / compositor will provide you a way to run a script on login, and that tends to have more straightforward / expectable behavior. \
Systemd is quite a bit more “raw” in that sense, and might not load some environment you expect to be valid, or might introduce a race condition that you may have thought to not be possible. \
I'll show you some tricks to help circumvent that, but if your wm has a mechanism for running a script on login, use it instead of an `OnStartupSec` systemd timer.

❗and here's the trick to symlink it and start it
❗now we start building the service file

❗run through shell trick
❗https://mysystemd.talos.sh/

# footnotes

{{hn(i=1)}} There's allegedly `anacron` that solves this issue, but it didn't work for me when I tried it.
