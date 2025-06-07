+++
title = 'optimizing paths'
date = '2025-06-06'
+++

The default paths that you start with, and the paths you might end up creating naturally, can be quite inconvenient to work with.

In your home directory, you probably have `Documents`, `Downloads`, `Pictures`, `Videos`, etc. \
To follow the semantic, you might create directories like `Programming`, `Notes`, and the like. \
All of them are pretty long words, and annoyingly they all start with an uppercase letter: neither of that helps if you need to type out full paths to things pretty often.

Unfortunately I do: my biggest usecase is [how I handle program setups](https://github.com/Axlefublr/dotfiles/blob/main/scripts/setup/dode.fish), but typing out paths comes up in all sorts of places. \
`~/Programming/Projects/Mine/Rust/2025` quickly becomes up exhausting to type in, over time. \
Tab completing paths *helps*, but has other issues that I'll go over in another upcoming blog post.

To minimize effort of typing in paths, as well as travelling to such paths, some time ago I renamed most of my directory structure to all be single characters, basically. Look at the tree below:

```
~
  r
    forks
    proj
    binaries
    backup
    info
  i
    tools
    e
      memes
      themes
      logos
      icons
    s
      screenshot1.png
      screenshot2.png
      screenvideo.mp4
  q
    torrent.torrent
  w
    random-download.jpeg
```

The core directories, that I'll be typing in the most often, are all single characters. But deeper directories are named normally.

This worked pretty well! Now to get to my screenshots directory, I simply need to type out `~/i/s`, rather than `~/Pictures/screenshots`{{fn(i=1)}}.
But there are two things that this approach misses.

First, it's ugly. \
Seeing `~/r/proj/harp` in my prompt just feels kinda bad :/ \
It's extra uglified by the fact that the core directories *are* single letters, but deeper directories aren't, so the path ends up looking unbalanced.

Second, it's a massive pain for search for. \
If you want to look for `Pictures`, you might need to filter out the normal word “pictures”, sure, but ultimately you're still narrowing things down somewhat. \
But searching for `i`…

I can do `i/`, but then I'm making the assumption that I immediately refer to another path fragment that is in `i`. \
I could `/i`, but then I'm possibly missing myself programmatically appending the path `i` to my home directory (think `home_dir + "i"`). \
`\bi\b` will show me way too many results.

So no matter how I search, I either have to filter through too many things, or I possibly miss some cases where I *do* still use the path I want to search for.

Additionally, I'm not doing zoxide any favors. \
The core directories might as well be impossible to zoxide to, since their queries would be so non-unique; but they're short specifically so that I *don't have to* use zoxide to get to them, to be fair. \
But then the deeper directories come back to being normal, common words, so zoxiding to them is *also* a pain, just of a different genre.

Yesterday, I decided to take on the grueling task of renaming and restructuring directories in my `~`, to handle all of my concerns.

I had multiple requirements for the new names:

1. Has to be a short name, but more than 2 characters. \
It will still be fast and easy to type in, while not looking too unbalanced and ugly.
2. Has to be gibberish. \
Making it way easier to search for precisely, and making it ideal for zoxide, as a nice side bonus.
3. Has to *feel* nice to type in. \
Following the point that paths are something I'll be typing out again and again in the future, I should have a comfortable time doing so.

Here's the tree that I ended up with:
```
~
  fes
    ack
    bin
    dot
    eli
    foe
    lai
    ork
    wks
  go
  iwm
    dls
    lwkc
    msk
    osl
    rnt
    sco
    tox
    twemoji
    vdi
  wlx
```

As you can see, almost all of the directories are complete gibberish. Except go I guess. They really couldn't've made it `.go` instead 🙄 \
I sat down, and naturally followed where my fingers like to go after typing each letter. \
After I type `f`, what letter feels nice to type in next? \
Then, I considered what directories are going to come after which directories.

`fes`, for example, will be coming after typing in `~/`; both ~ and / are on the right side of the keyboard (for me), so that's why `fes` is so left-focused. \
All the directories inside of `fes` will come after `fes`, obviously, so now I optimize their names according to that. \
After a heavy left-side, I now introduce the right side keys quite a bit more.

I figured out what felt good by repetitively typing in the full path to the directory, and feeling it out. \
For example, `fes` was first `int`!{{fn(i=2)}} \
But after typing it out a bunch of times, I noticed that pressing `n` so close after `~/` simply didn't feel that good; which is a big nono for such a core, commonly typed directory. Additionally, `int` is a real thing that may come up in searches, as a false positive, so that's another reason to not use it.

After making the highest level directories, I went on to rename a bunch of directories inside of there. Look at a tree again, but now with context of how the directory used to be called.

```
~
  fes <- r
    ack <- backup
    bin <- binaries
    dot <- dotfiles
    eli <- auto
    foe <- info
    lai <- proj
    ork <- forks
    wks <- wks, actually. this one hasn't changed
  go
  iwm <- i
    dls     <- didn't exist
    lwkc    <- we don't talk about this one
    msk     <- mus
    osl     <- e
    rnt     <- q
    sco     <- s
    tox     <- tools
    twemoji
    vdi     <- v
  wlx <- w
```

You can see how a bunch of the directories took their previous name in some form, and either expanded it or collapsed it, making sure it's gibberish. \
`tools` is a common word -> `tox` is shorter and not a real thing. \
`documents` is another word -> `dls` is unique and nice to type in. \
`backup` may even genuinely be used as a directory name all over the place -> `ack` now has a subdirectory named `tua` which is hilarious.

All of these directories I can now effortlessly zoxide to, *or* type in manually without complaining about it. I found a great balance.

A question you may have now is "how the hell will you remember all of these?" — that's why the naming stage is so important to take seriously. \
When creating a directory name, I let the purpose of the directory guide me into naming it naturally. \
Basically, `ork` *feels* correct to me to contain my forks, and so is easily memorable. \
`lai` is nonsense, but it looks beautiful to me; *that* is what makes it makes sense to me, for the purpose of storing *my* projects. \
Because `fes` used to be `int`, `foe` is now really memorable to hold “info” — the path `int/foe` is peak comedy to me. Similarly to `ack/tua`.

This is why I cannot give any specific suggestions to what *your* directory names should be; what feels correctest to *you* will be similarly heavily personalized. I hope I could inspire you to rethink your directory structure, to make it more *enjoyable* to traverse and look at.

# footnotes

{{hn(i=1)}} I'll talk about zoxide later, don't worry. Part of god's plan.

{{hn(i=2)}} The subdirectory `eli` comes from a pun based on the upper directory being named `int`. If I named a future project `gents`, I'd have a path `~/int/eli/gents`. Which is of course hilarious.
