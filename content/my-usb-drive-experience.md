+++
title = 'my usb drive experience'
date = 2024-11-15
+++

I hate buffering, so I download practically every youtube video I watch using yt-dlp. \
That's all fun and great, however my 256gb laptop is slowly running out of space! \
Well... that's a bit of an overestimation: I have hours upon hours of content downloaded and am still at only 71% usage, but that number freaks me out regardless!

You may reasonably suggest to upgrade my SSD to something like 1 terabytes. \
However, I find it boring! Often times when I solve some issue, I do it with a completely different agenda in mind.

{{ hr(id="suboptimal") }}

If I upgrade the SSD, I just end up never having to worry about storage again (I keep everything pretty clean, so 1tb is more than enough) and learn *nothing*. \
But if I try to solve the issue with a *usb drive*, now *that's* interesting.

I know Linux's mechanics for mounting a drive, but mostly on paper — I've done them before, but they aren't something that I do often enough to put deeply into my workflow. If I *have to* use a usb drive, I'll learn about all the steps surrounding it more deeply, so I get a "learning benefit" by trading in effectiveness of the solution, lol.

{{ hr(id="simple-parts") }}

As I've made clear, I don't have deep knowledge, but let's go through the simple parts that you may want to know, if you didn't already!

On windows, new drives appear separately, as new drives. With their own letter and everything. If you get beyond drive `Z`, your computer explodes. Simple stuff.

Linux does this differently! You don't get another `/` or anything like that — *you* get to decide where the (usb) drive lives by *mounting* it. You say to the filesystem: "hey, act like this usb drive is located here".

{{ hr(id="mounting") }}

```sh
sudo mount /dev/sda1 /mnt/usb
```

Looks like a pretty simple command! What was most non-obvious to me in it, is the *directories* passed.

"What in the living and dying hell is `/dev/sda1`?" — that is our usb drive! Well... saying that is a bit misleading. That name is not at all guaranteed and might be different for you, and that's if the drive got recognized to begin with!

{{ hr(id="lsblk") }}

So, let's call `lsblk` in our terminal and look at the output to try to find our drive:

```
NAME        MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
sda           8:0    1 230.5G  0 disk 
└─sda1        8:1    1 230.5G  0 part /mnt/usb
nvme0n1     259:0    0 238.5G  0 disk 
├─nvme0n1p1 259:1    0  1000M  0 part /efi
└─nvme0n1p2 259:2    0 237.5G  0 part /
```

Don't worry, it's confusing to me too! -1 clue what `MAJ:MIN` means, and `RM` is a wonder to me as well. Forget about `RO`. Anyway, I'm going to focus on the part that's going to be useful to you first.

In the `NAME` column, thingies are shown in a tree structure. \
`nvme0n1` is the name of my SSD drive! and `sda` is the name of the usb disk. Yipee 🥳

{{ hr(id="tree-structure") }}

However, there's still this tree structure. This confused me, but now I finally realize what's happening: the items that are branches (with the `└` before them) are *partitions* of the above drive. *These* are what you should be pointing at when trying to specify a drive somewhere, *not* that parent item.

So, what we did so far is the following: inserted the usb drive into some port, executed `lsblk` and found a drive that isn't our SSD. Then looked at all the partitions (branches) it has (may have more than 1 potentially) and took *that* as the thing that we'll be calling our usb drive.

{{ hr(id="arbitrary-mount-point") }}

That `/mnt/usb` directory is actually pretty arbitrary! The `/mnt` directory is a conventional place where you can **m**ou**nt** your various drives. \
It is not the only place however, I *believe* ext4 (the linux filesystem you're probably using) allows you to mount things anywhere.

So, in the `/mnt` directory I created a directory called `usb`. Just makes sense to name it that way, you can name it whatever you want!

Keep in mind that `/mnt` is restricted, so you'll have to create `usb/` with elevated privileges:

```sh
sudo mkdir /mnt/usb
```

{{ hr(id="restriction") }}

Because `/mnt` is restricted, `/mnt/usb/` is going to be restricted too. Considering that this our lovely personal usb drive, it makes sense to *own* it.

```sh
sudo chown username:username /mnt/usb
```

Viola, now the directory is actually ours 🥰. I sure hope you replaced `username` with your *actual* username!

{{ hr(id="we-can-start-now") }}

After explaining what the directories mean, now `sudo mount /dev/sda1 /mnt/usb` makes a bit more sense. \
We finally can start actually using our usb drive, as if it's just a normal directory on our filesystem!

{{ hr(id="massive") }}

The 61 gigabytes of content I had downloaded, I now started moving to my shiny new drive.

Quickly issues started to arise, and my mirror rolled its eyes 🙄 when I said "I told you this would be a learning experience!".

Sixty one gigabytes of data is quite a lot! Understandable that it would take some time to move. But it was getting ridiculous! \
In my very rough count, it took TWO whole minutes to move a single gigabyte. The drive I bought is usb 3.1 so I was flabbergasted, but brushed it off as "damn I guess usbs are that slow still". \
The last time I needed to use a usb drive *not* to install linux was back on windows, so I had no recollection of a good transfer speed. \
However, in an interesting turn of events, this turned out to be a chehov's gun. Yes I am making a cliffhanger in a blog post, what about it >:D

{{ hr(id="fine") }}

I don't need to move a whole ass 61gb of data often, if at all, so this slow transfer speed seemed to be an okay-ish tradeoff for the extra space I now get. \
My internet speed will be the bigger bottleneck anyway.

After ~4 hours (or so it felt), the moving *finally* finished. I excitedly went ahead to download more youtube videos, now to the *usb drive*, rather than my SSD.

Exciting! \
As I download the new videos, I decide to watch one I already have downloaded, at the same time. \
After a while, I notice a *horrifying fact*... There's buffering now! \
What the fuck? The whole reason I download all youtube videos I watch is to *avoid* buffering.

{{ hr(id="fucked") }}

So I guess write speed is a bottleneck in the place that matters more: my watching experience... 😭

Well, let's think about this: currently I have 6 videos downloading. All of them are written to the usb. \
I'm not sure how read vs write conflict, but I'm sure writing that much introduces some congestion. \
I guess I just have to download videos while not watching them at the same time! \
That's kinda bad, but I can easily get used to it, no big deal.

After my downloads all finished, I decided to test by watching another downloaded youtube video. Surely now the buffering is gone; there's no writes coming into the usb! \
Oh my god... I am horrified to discover that I *still* have buffering... It's *maybe* not as bad, but this thought may just be powered entirely by bias.

Let's start another chehov's gun here, and talk about something that *seems* unrelated.

{{ hr(id="lights") }}

I despise when hardware, like my laptop, earphones, headphones, speakers, power switches, phones, have some lights on them. \
The reason for them to exist is to tell you information, like the charge percentage, connection state, on / off position. \
However, I'm autistic! Those random lights can be a real bother when I'm trying to sleep, and yet my room lights up.

When I first inserted my new usb drive, I was horrified to find out that it has a light on it too... \
I guess I should've checked for that, but the design of the usb drive had nothing to suggest a light; \
matter of fact, the light is *inside* of the usb, in a sorta awkward fashion.

I would never expect that I would not only *like* this light, but also find it useful.

I eventually naturally noticed that it stays solid some times, and blinks other times. \
This *has to* mean something, right? Turns out that if it's blinking, some io operations are being done with the usb drive; if it's solid, none are happening.

Nope, surprisingly enough I didn't read the manual on the usb drive (`HS-USB-M220P/256G/U3`); I discovered this behavior via another way:

I remembered the `sync` command on linux. It flushes the read/write data to make sure it reaches its destination, *finalizing* all the read-write caches.

As I was watching that youtube video I downloaded, with no other downloads happening at the moment, the light *was* flashing. \
Fair enough, it's reading from the drive to let me watch the current video. So I close it, but still see it flashing even after waiting for a couple of seconds. \
I do `sync` and eventually the flashing goes away 🤯 \
So *maybe*, the weird buffering issue happened because of write caches staying for too long?

Nope. Cool guess though.

We handled chehov's gun #2, but didn't actually move to the solution.

Let's check up on chehov's gun #1! What about those super slow write speeds anyway?

{{ hr(id="specs") }}

After not being to fix this buffering issue, it made me think: \
I bought my laptop 2-3 years ago, for a pretty solid price, *new*. \
How the fuck would it only have usb 2.0 ports? \
That sure sounds unlikely!

So this time I *do* google the [specs of my laptop](https://www.asus.com/laptops/for-home/everyday-use/asus-m515-amd-ryzen-5000-series/techspec/). \
Europe! I *do* have a usb 3.x port!

You might reasonably ask "how the hell do you not know that already?"

Good question!

I noticed a long time ago that usb 3.x ports are colored blue, to make them easy to tell apart from usb 2.0. \
This seemed so consistent, that I simply ignored one of my ports as a possible solution.

I have nothing to lose by trying to prove myself wrong, so I insert the usb into the one port I haven't tried, mount it, and try moving a big ass video file, that would usually take 2+ minutes to move.

Well holy shit! It's moving *waaayyy* faster! Yes! This is definitely a 3.x port! \
(however it is still massively slower than what I expected out of 3.x).

# aha! chehov's gun #1 solved everything!

Right?..

Wrong 😭

Sure, moving stuff is now way faster, and I got a sense of regret due to the 4 hours of useless extra waiting, \
but fascinatingly enough, I *still* had weird pauses in mpv.

And as always, A™ Configuration™ Option™ saved the day.

Turns out if you set `cache=yes` in your mpv config file, it will automatically prefetch the video you're watching wayyyy forwards, completely eliminating the pauses (that I call buffering), ***even*** if you're also downloading like 6 videos at the same time. \
God I love mpv 😩 /srs

Yipee 🥳

# would be better to know about this prior, wouldn't it?

Nope, not really!

I decided to buy a usb drive specifically to run myself into issues like all of these, and *learn* things by solving them. \
I succeeded in that exact goal SPECTACULARLY. The decision went *exactly* according to plan.

First, I now know that I have a usb 3.x port, and that the other two are usb 2.0.

Then, for once I like a light in my hardware! \
Thankfully, it turns off when the laptop goes to sleep (thank god). \
I like it now because of how nice it feels to *have* the information of io operations given to me at all times. \
If I notice the light blinking at a time where I don't expect, it's something that I can look into / investigate, rather than not know at all. \
If you saw my window manager bar, you'd say "yep checks out". (I have like a billion widgets)

Knowing the io operation information also made me *accept* a lesson I always thought to be unecessary bullshit.

Since childhood, back on windows, people around me always said to right click the usb drive and click "safe remove". \
I did it at first, of course, but after a while I noticed that it's just completely useless. Does seemingly nothing.

*Now* that I actually see the io operations by the light, I won't just pull out the usb all willy-nilly: it's *clear* that I would be fucking over *something*.

So what I now do is first `sync`, and then `sudo umount /mnt/usb`. \
For a shortcut that does *both* at the same time: `sudo eject /mnt/usb`.

# conclusion

I sure learnt a lot! And still have the opportunity to learn more: experimenting with the drive's filesystem, figuring out an auto-mounting solution (requiring sudo every time will, I'm sure, get annoying), or maybe another problem I don't yet realize I have!

And all these reasons are why it's sometimes reasonable to go for the worse solution, if you feel that it will lead you to something more important! :3
