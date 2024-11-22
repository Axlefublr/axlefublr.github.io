+++
title = 'magazines'
date = '2024-11-03'
+++

I came up with a concept that completely revolutionized a *lot* of my workflow.

I introduce to you: magazines™!

Each magazine is just a normal file that has a one letter file name, from a to z, from A to Z, and from 1 to 9. \
All magazines are located in a single directory called `magazines`.

> The name doesn't really have meaning, I just happened to be obsessed with guns at the time of coming up the concept

I have different *actions* I can do on any magazine. An "action" is just some shell function that interacts with a file in some way.

And finally, I have chord mappings that make me pick an action, and then a magazine to act on. \
All the magazines use either a letter or a digit for their filenames, to mimic the hotkeys I use to choose them.

All action picking hotkeys use *left* shift + a *left* side key. This is because when typing, I will always use the right shift for left side keys, so I get to have very accessible hotkeys specifically for magazine actions. \
After I press the action hotkey, I press a letter (or digit) key, to pick the magazine I want to execute the action on.

This is a pretty simple concept! But it's so flexible *specifically* because it's so simple.

Considering that all actions *just* act on some filepath, let's go over the actions I can do!

1. use `rofi` to type in a line of text, and overwrite a file with that text (<kbd>lshift+alt+s</kbd>)
2. 1, but append the line rather than overwrite with it (<kbd>lshift+s</kbd>)
3. copy the text in a file to my clipboard (<kbd>lshift+c</kbd>)
4. truncate the file (remove its contents, but don't delete the actual file) (<kbd>lshift+r</kbd>)
5. 3 + 4 — this is basically a "cut" action (<kbd>lshift+alt+c</kbd>)
6. take the contents of my clipboard, and overwrite a file with them (<kbd>lshift+alt+v</kbd>)
7. 6, but append the contents rather than overwrite with them (<kbd>lshift+v</kbd>)
8. use `rofi` to pick *a line* from a file to *remove* and copy to my clipboard (or, to *cut* to my clipboard basically) (<kbd>lshift+f</kbd>)
9. use `rofi` to pick *a line* from a file to copy to my clipboard (like 8, but doesn't remove the line) (<kbd>lshift+alt+f</kbd>)
10. open a floating terminal, in which the file is opened in helix, to get the full power of my editor in a fast way (<kbd>lshift+d</kbd>)

> Further in the gist I'll refer to actions by their shortened name, with a superscript number next to it, that specifies the index in this list.

All actions that modify a file, also *commit* it with git — the `magazines` directory is a git repo. \
This way, I have a *very* easily traversable history, that doubles as a backup. \
As long as something *has been* in a magazine, I will be able to restore it if necessary.

Commits are done automatically by all actions that modify a file; however I can still modify magazines elsehow. \
I have hotkeys in helix to open each magazine in the already existing editor session (different from the 10th action, because I'm not opening a new floating terminal window). \
When editing the magazines like this, it's not an action so can't be a convenient trigger for an autocommit. \
That's why there's an 11th action that *just* commits a magazine.

All the leftover changes that *still* didn't get committed, are collected together into a single commit, daily. \
So at the absolute worst, I have a day old backup for all magazines.

Let me go through some of my actively used magazines and their purpose, to explain how this system helps me keep track of things in an easy way.

The `r` magazine stores all things I want to read. "read" is used pretty loosely here, as the file contains a bunch of different stuff I basically want to *get to*, while not necessarily being a "task" that I can *do*.

Here's part of what it contains currently:

```
!# gh cli (gh-issue-develop)
!& Hugo
# pandoc
# string-replace
$ loago-based workflow
$ magazines
% how to remove background in krita
% can you "close all other windows" to close the main window from an overlay window in kitty?
& qalculate
& zbarimg
pcre or whatever regex
the sliding window technique
https://matklad.github.io/2021/07/09/inline-in-rust.html
https://www.howtogeek.com/how-to-see-beautiful-git-project-stats-in-your-terminal/
```

This seems like a hotpot but it's pretty intentional. \
`#` marks *docs* that I want to read (with the part I left off on written down in `()`), `$` is gists / blog posts that I want to write (`magazines` is this one!!), `&` is for *programs* that I want to discover, `%` is for questions I want to discover, figure out, or take in.

The two questions I have there seem simple, but they may stay there for a while because they're more loaded questions than they seem. \
I'm sure there's more than a single way to remove the background in krita, with each being useful in a different situation — I'd want to focus on learning this properly. \
Even more the case with [kitty](https://gist.github.com/Axlefublr/260f653c25c34c3ed6147a6638fc6512) — trying to solve this question might move me to come up with a different window managing concept that I'll then spend three more hours on. \
Never underestimate a question, especially if you're autistic like me.

I usually use the append² action for this magazine.

With [vimium c](https://github.com/gdh1995/vimium-c), I can copy links I see on the screen to my clipboard, using my keyboard. \
The various rogue links you see without a section header are usually added by copying a link that way, and using the append clipboard⁷ action.

Sectioning things is pretty nice, but has an additional benefit that I haven't yet made clear. \
The commit¹¹ action has extra functionality to it: I can pick filepaths that are automatically *sorted*. \
The section headers intentionally use ascii symbols that go before letters, so that they would appear first in the file after it gets automtically sorted.

Going in and editing the code for the commit action every time I want to autosort another magazine would go against my principles. \
This is why I use *another* magazine to solve this!

Magazine `O` contains all the filepaths that I want to automatically sort:

```
~/.local/share/magazine/l
~/.local/share/magazine/r
```

So if I decide to autosort another magazine on commit, I can just add its filepath there! :D

This autosort behavior explains the reasoning behind `!` before some section headers: `!` is the first printable character in ascii, excepting space, with its decimal index being 33. So it's guaranteed to come first. \
I use it for reading tasks that I want to get to *first*, before any other ones.

There is also magazine `P`, which is similar in concept to `O`: filepaths listed in magazine `P` are automatically made *unique*. \
If a magazine path is listed in magazine `P`, there will never be duplicate lines in the former.

I could opt to join `O` and `P`, but I might want to sort a file without deduplicating lines, and may want to deduplicate lines without sorting (which you can achieve with this monstrosity: `awk '!seen[$0]++'`), so I keep them separate.

The above examples are magazines that are fairly dynamic and moving, but there's use in magazines that are fairly static, and are made mostly to access the information faster, than necessarily change it often.

Magazine `u` contains a bunch of cool looking special symbols and nerd font symbols, that I may use in the future. Here's a small chunk of it:

```
󱞭 󱞯 󱞱 󱞳 󱞵 󱞷 󱞹 󱞻 
󱞡 󱞣 󱞥 󱞧 󱞩 󱞫 󱞽 
    󰁕  
    ⮌ ⮍ ⮎ ⮏  
← 󰹹  󰒟 
󰚪 󰘲 󰕍 󰓦  ↪ ➜ 
󰒊 ▶ 
```

The only action I may conceivably do with this one, is to edit¹⁰ it. And even then, usually from an already existing helix instance. The benefit is yet still huge, as it's very easy to get to this file to add another cool unicode symbol.

Here's another magazine that is quite directly opposite: magazine `o` stores the command output of the last command I ran in my rofi command runner.

Usually, the output of my rofi command runner is shown in a notification that stays for 3 seconds. That's plenty of time for the vast majority of usecases, but I *may* want to look at it for longer. Blammo — edit¹⁰ action.

That's fairly convenient as is already, but the real power lies in the copy³ action. Any time I execute a command, I can *choose* to copy its output, if I need it. \
So, I don't have to preemtively plan to pipe the output into `xclip`, and don't have to rerun the command once I realize I want to pipe it into `xclip`. I *just* use the copy³ action.

Another power I get for free, is that I can choose a line to copy⁹ from the command output.

I didn't consider this as something I'd want to do with command output — it wasn't forethought. \
It just so happened I needed this action for something else (magazine `F` that stores cool fonts) and now the action is integrated into the *magazine framework*. \
So with magazine `o`, and many other ones, I end up having editing flexibility that I didn't plan for that magazine specifically. It's a big part for why this system has been so successful for me.

I create all the hotkeys for all the actions, and all magazines, myself. There are like 900 or so lines of yaml to make them all using xremap. \
Because I don't have some complicated system to make the mappings, nothing is stopping me from making some action for some magazine behave slightly differently, to be more useful for that magazine.

Mag `l` is my "linker". It contains all the links I want to store in this format:
```
!general my youtube channel — https://www.youtube.com/@Axlefublr/streams
blog reasons I still love fish shell — https://jvns.ca/blog/2024/09/12/reasons-i--still--love-fish/
doc ankiconnect — https://foosoft.net/projects/anki-connect/
general phind — https://www.phind.com
gist why I hate kitty — https://gist.github.com/Axlefublr/260f653c25c34c3ed6147a6638fc6512
mat gun sounds — https://samplefocus.com/tag/gun
meme I have done nothing but teleport bread for three days. — https://www.youtube.com/watch?v=NE1-dKc6R_I
music hatchetsaw, hauntgat, fleshwound — country side massacre — https://www.youtube.com/watch?v=H4Hw-ARuEa4
plug nvim harp-nvim — https://github.com/Axlefublr/harp-nvim
proj Axlefublr/helix — https://github.com/Axlefublr/helix
```

First goes a "header": `general` / `blog` / `music` / etc. Then the title of the link. Then a `—` (long dash, U+2014). Then the actual link.

I used to use the append² action for this in the past, but found it annoying to have to put in a long dash *and* paste in the link every time, considering that it's the *only* thing I'd want to do anyway.

I created a "subaction" meant to be the append² action replacement, but *only* for magazine `l`. The only difference, is that after I type in the input (link title), it appends a `—` to it and the link (which is in my clipboard), and *then* adds it into the file.

That makes it really non-laborious to add in new links! \
As well as showcase how flexible magazines are, considering that I was able to make a custom action for something like this.

Now, there's a reason I want a specific format here to begin with. I don't actually open this file directly that often. Magazine `l` is used as a "data file".

All the links are stored in it because I have a global hotkey that *parses* through them all, lets me search by title, and then either opens the link or copies it to my clipboard.

So, sometimes magazines *are* the behavior, other times they're an *interface* to interact with a data file conveniently, so that it could be used by something else.

I also have a special symbol picker in magazine `e`, and the idea is basically the exact same:

```
¡ a1 reversed exclamation
⁷ 2077 top superscript 7 seven
↕ 2195 arrow down & up
🐄 1f404 cow
🔇 1f507 muted speaker
🤢 1f922 vomit
```

Easier automatic adder, that takes the symbol from my clipboard, figures out each character's unicode (try `printf %x \'🐄`), and asks me for a title. Then a hotkey that lets me search by all of that, to copy just the symbol to my clipboard, so I could paste it wherever I need.

That's about it when it comes to the explanation of the idea! Hope you got motivated to implement something like this, or better yet — something that makes even more sense for *you*. Right now I'm going to continue explaining a bunch of my other magazines and what I use them for, to maybe help you get ideas.

---

Out of these magazines: `abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789[];',./{}:"<>?`, \
I'm actively using these: `acdefijklmopqrsuvwxACDEFHIJKLOPQRSTUVWXYZ134[];:`.

Yes, really, that many! I'm thinking of possibly adding hotkeys with <kbd>alt</kbd> in the future, once I run out of magazines. Then <kbd>ctrl</kbd>, then <kbd>shift+alt</kbd>, then... etc. That would in theory increase those ~900 lines of yaml to be like, three million! Oh well :>

<kbd>a</kbd> stores "all the possible things I can do" — helpful to take a step back, breathe, and calmly decide what next to do, rather than hurrily and anxiously jump around in my mind.

<kbd>c</kbd> stores copypastas I want to be easily accessible. Here's one of them: «when making australia God said "only a spiderful" but then pulled out a comically large spider».

<kbd>d</kbd> diary that gets automatically emptied every day. Because all remaining changes get automatically committed at the end of the day, I have access to all of my past's diaries, without it cluttering up in one ginormous file. I don't really ever read my diaries from that past, that's why.

<kbd>f</kbd> similar to <kbd>a</kbd>, but I store *specifically* the "next" things I want to do. Here's what it has now:

```
ff: !' is ctrl+l, +!' focuses page by probably clicking
<c>, cda, cdb, cdab, codeblock, code, anki in not subheaders
qalculate
life drain anki plugin https://ankiweb.net/shared/info/715575551
css flex
blog
https://ankiweb.net/shared/info/206062158
anki searching
```

Very helpful with keeping me on track!

<kbd>k</kbd> stores links to youtube videos I want to download, <kbd>K</kbd> to *longform* videos, <kbd>i</kbd> to asmr videos. I have a script that takes a couple (dynamically settable) links from one of these three files, and starts downloading them, removing them from the magazine.

<kbd>j</kbd> is special-cased to instead be `~/prog/dotfiles/project.txt`. `project.txt` is a file that gets automatically created for all of my projects, and I use it to organize thoughts of what I want to do in a given project. \
<kbd>J</kbd> is special-cased to let me *choose* a `project.txt` to act on. Reason why <kbd>j</kbd> is made more accessible, is that I *really* like to configure so I interact with the dotfiles project file the most frequently.

<kbd>m</kbd> stores long-term project ideas that I'll definitely do Eventually™.

<kbd>p</kbd> my proxy's information. I can quickly go in and copy the address, for example.

<kbd>q</kbd> my hardware specs. Even includes the specs of my physical body, lol.

<kbd>v</kbd> important events that I want to be reminded of yearly. I have a script that runs every day to check this file, and tells me stuff like "3 years ago: started programming". Here's the format:

```
21.09.01 — started programming
22.09.24 — started using vim
23.08.06 — installed EndeavorOS
24.02.21 — she/they
24.08.18 — switched to helix
```

<kbd>V</kbd> is the same thing with the same format, except it's *not* checked daily; it's for event dates I want to have access to, but don't care to be reminded of.

```
23.07.26 - installed Debian
24.02.22 - switched to pure neovim
24.03.11 - 16gb ram
```

<kbd>Q</kbd> is also similar to <kbd>v</kbd>, but doesn't specify a year — mostly for storing birthdays and getting reminders of them.

For reminders of things that will happen once, or monthly, or yearly (mostly payments and meetings), I made a python script that parses <kbd>T</kbd>.

<kbd>w</kbd> is the short-term shopping list, and <kbd>W</kbd> is the long-term shopping list. \
These make more sense once you realize I have a telegram bot that lets me easily interact with magazines from my phone.

<kbd>x</kbd> contains today's "habit tasks". My daily tasks alternate: one side is stored in <kbd>\[</kbd> and the other in <kbd>]</kbd>; each of them happens every other day.

One thing I have in <kbd>\[</kbd> is to read through <kbd>D</kbd>, which stores words / mindsets that I want to implement into myself, and so repeat every two days:
```
split the scary into "not very"
laborious
stellar
:o
:O
・>・
```

<kbd>Z</kbd> contains package names I installed with `pacman` (meaning, from one of the arch repos), \
<kbd>X</kbd> for those installed from the AUR.

In my system setup script, both of these magazines are used to install all packages I want.

<kbd>C</kbd> stores packages that I want to be deleted the next time I update. This makes more sense to exist once you realize the workflow of filtering⁸ <kbd>Z</kbd> or <kbd>X</kbd>, and then appending the clipboard⁷ to <kbd>C</kbd>.

<kbd>E</kbd> is actually a big ass list of all unicodes for my other, slower but more complete unicode symbol picker.

<kbd>L</kbd> contains commands to always be in my rofi command runner history, <kbd>H</kbd> for the dynamically generated history.

<kbd>R</kbd> repository paths, that should be automatically `git push`ed daily.

<kbd>U</kbd> a big ass list of words I use for `ttyper`, to practice touch typing. Which I never do anymore, but still.

<kbd>Y</kbd> my youtube channel subscriptions, because I don't trust google to keep that information.

<kbd>1</kbd> consistently used as "store something short-term".

<kbd>3</kbd> a bunch of things make "notifications" into here, like my various event notifiers, for example.

<kbd>4</kbd> github notifications go here.

<kbd>;</kbd> by the time I finished writing this gist, the `$` writing tasks from <kbd>r</kbd> moved here.

<kbd>:</kbd> similarly, `%` "processing" tasks from <kbd>r</kbd>.
