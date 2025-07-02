+++
title = 'harp'
date = '2025-06-02'
draft = true
+++

Travelling to files in text editors is very slow at best.

There are four main ways to travel to a file from your text editor. Let's discuss them.

# fuzzy picker

Your telescope, or snacks file picker; your helix file picker. Your vscode <kbd>ctrl+p</kbd> fuzzy file picker, even. The classic.

You search through a list of all files in the current directory, using a fuzzy search algorithm — you can type `aaa` and it will match `banana`.

This approch is what neovim users claim to be the silver bullet to file traversal. Something that *solves* travelling between files quickly and efficiently. \
“I can get to any file in just a few keystrokes!” is a notion I reject categorically. \
Efficient fuzzy searching is non-intuitive; it's something you actively learn to do.

Think of lsp suggestions. Say you want to complete a function with a really long name: `here_is_my_longly_named_function`. \
The efficient approach to matching it is probably going to be `himlnf` — type in the first letter of each subword, to almost guarantee that it will be the first match. \
Let's not kid ourselves; this is not intuitive. \
Despite me knowing the `himlnf` trick, I'll still *naturally* start typing `here_…`, and then complete the name of the function once it appears in my lsp suggestions. \
Fuzzy lsp suggestion matching is something I have to make the *active decision* to do. Or perhaps, something that I would have to *train* myself to always do. I don't think I'm alone in this.

I'm not saying fuzzy matching is *bad*. Hell no! I'll pick fuzzy matching almost always over substring matching. \
But this is not my point. My point is that *efficient* fuzzy matching spends mental cycles; in most cases what you'll end up doing is just typing in a substring rather than a heavily optimized fuzzy matcher.

Coming back to the fuzzy file picker. \
Most files in a project you probably don't have to visit often, so simply using a substring is no big deal. \
But there will be some files you interact with over and over again, where using simply a substring over and over again will become very painful. \
And *that's* where you decide to put in the extra mental effort, and figure out an efficient enough, and nice-to-type enough fuzzy matcher.

“I can get to any file in just a few keystrokes!” — this is where we disagree: my definition of "few" and your definition of "few" seems to be quite different. \
Or rather, if "few" is to be taken literally, that's an unacceptable cost to me.

From experience of coming up with these good fuzzy matching strings, they tend to be 4-5 characters. \
We add in the extra 2 keys pressed to invoke the picker to begin with, and pressing <kbd>Enter</kbd> to accept the result. \
That is 7-9 keys pressed to get to a single file. SEVEN TO NINE ENTIRE KEYS.

Alright fair enough, mister seven-to-nine keys man (what a weird nickname you have, too), I don't mean to yuck your yum. \
You got your perfect sequence of keys to press to get to a specific file, and now you're golden. \
Sure, it's kinda a lot of keys to press, but you got it in your muscle memory now, and you're super quick at entering those now!

Until tragedy befalls you… You create a new file 😨 What horror! \
And now it's *that* file that appears first in the result list, after entering your perfect fuzzy matcher, \
rather than the file that you were aiming for!

And now you sheepishly refine the query you got so used to, to match the old file correctly again. \
You might've needed to add one or two extra keys to press for the query, too. \
And the scariest part? In the future, this may happen again… \
Worse yet, there will be files that you are **unable** to place as the first result, making you always have to press <kbd>Down</kbd> or <kbd>ctrl+p</kbd> or what have you.

Obviously I'm being dramatic here, but this both conveys my point and is probably fun for you to read :) \
Fuzzy searching to travel across files you travel to *really really* frequently is both inefficient *and* inconsistent. Reasonable enough for rare files though.

Aside from the optimal query being *long*, it has a pretty big chance to not be enjoyable to type in anyway. \
It's common that file structures of projects are made in such a way, that you end up needing to match really annoying paths. \
And you can't exactly optimize your file structure to fit the fuzzying need; at least not for every project.

# file explorers

File trees, or file lists, like [oil.nvim](https://github.com/stevearc/oil.nvim) (my beloved) — UI that presents the files visually, and tends to allow you to do file operations, like creating new files, deleting them, renaming them, etc, but also *opening* them.

Where these shine is *file operations*, rather than being a fast way to *get* to some file. \
“Open file tree in the parent directory of the current buffer” very much comes in handy to jump between multiple closely related files. \
Also, using a tree / file list helps to get a better grasp of the overall project structure; something that's hard to do with a fuzzy file picker.

But actually ←↓↑→ing through directories and files to jump to some file is of course inefficient — it's more so something you do when you aren't sure what file you even want. \
In other words, file explorers are good, but for other usecases; not for fast and efficient file traversal.

# lsp actions

Go to: definition, declaration, type definition, implementation; Open fuzzy picker for: symbols in the current buffer, the entire project, outgoing calls, incoming calls, references.

~~All~~ most of those are instrumental for a normal dev workflow, and of course I use them actively.

But the reason why they don't solve the entire question completely, is because they are all lsp reliant. \
You have to have an lsp for the given language, and set up to actually work; also, it needs to implement the above lsp actions. \
Then, your *editor* needs to be able to interpret them, too (helix doesn't have incoming/outgoing calls, for example).

And even after all of that, lsp tends to take quite some time to start up. \
In rust, especially, by the time the lsp gives you the ability to use its actions, you probably would've already travelled to the file and place that you wanted to get to, by using any combination of the other methods.

Once the lsp is started up and working, the usecase question still exists: you don't always want to jump to a type, or function, or a part of the source code, even. \
For its intended usecase, lsp actions are very helpful and powerful, but definitely not a generic cure for every usecase.

# :edit command

`:edit` in neovim, `:open` in helix; basically, opening a file by typing in its relative (or full) path directly.

The big part of this method is the existence of <kbd>Tab</kbd>.

Say you want to complete the path `~/programming/dotfiles/colors.css`. \
You start typing `~/p` and press <kbd>Tab</kbd>. \
In your home directory, you also have directory `progesterone`, so you get shown both results; \
Pressing <kbd>Tab</kbd> once accepts `progesterone`, pressing it twice will accept `programming`. \
Basically, you have to rotate through the results.

However, if you typed `~/progr` and pressed <kbd>Tab</kbd>, now you *don't* have to choose: there is only one valid match for the substring `progr`! And it's `programming`. \
So, to complete the path `~/programming/dotfiles/colors.css` most efficiently, I'd type `~/progr<Tab>/d<Tab>/colo<Tab>`.

You can pretty quickly realize how it's beneficial for a path to become unique quickly; `Downloads` and `Documents` both existing make me kinda mad for this reason. \
While you can [optimize directory names](@/optimizing-paths/index.md) like `Downloads`, `Documents`, etc to be faster and easier to tab complete (or type in), there will ultimately be some directories that you cannot / should not optimize, like `~/.config`, `~/.local/share`, `~/.cache`. \
They unfortunately also carry wonderful semantics, that make a lot of sense to make use of. And of course, they are used by all of your programs! But getting to them is a pain! :c

With perfect fuzzy queries, you at least have a bit more space to move around in. \
With tab completing a path, you need to complete each part of the path separately, rather than being able to match the entirety of the full path with some perfect fuzzy query. \
Considering that it's not often that tab completing supports fuzzy matching, it makes the most sense to get used to (and internalize) perfect tab queries for the paths you might want to travel to often.

Why even use this method ever? \
Fuzzy matching file pickers generally open within your current working directory, and if you're in `~/fes/dot` rather than `~/.config`, you won't be able to pick a file that's in `~/.config`. So that's the main reason you'd ever use `:edit` over fuzzy picking. \
Once again, this solution is not the bees knees; matter of fact, it might be the absolute worst approach, depending on how you look at it! :»

# bookmarking

As you find pathing pain points, you might be led towards a certain idea.

You find yourself editing some specific config file over and over again; \
`cd`ing into some same directory. \
So you decide to make a mapping in your editor to open that file, \
And an alias in your shell to `cd` to the directory.

Yay! Now a pain point is gone :» \
You can navigate to things you navigate to often, quicker and more comfortably. \
But there's an issue: this method doesn't scale that well.

First, you start with having aliases for `~/.config`, `~/.local/share`, `~/.cache`. All directories that you will for sure be jumping to over and over again. \
But then you suddenly realize you need to jump to a directory `~/.local/share/Steam/steamapps/common/Enter the Gungeon/mods`.

Navigating into your config (which you have on a mapping, let's say), finding the new place where you'd like to add the alias ([unless that's not a problem for you](@/consider-sorting/index.md)), and typing in that path + the syntax for making an alias is undeniably a bit of a workflow break. \
It is something you stop to do, and at best will spend 20 seconds on. Then “wait, what was I doing again? ah right”. I experienced this many times.

So you might be led to making some hotkey that creates an alias for you, and also adds it to your configuration. But while that solves the stoppage issue, it has another one.

You create an alias for that enter the gungeon mods directory, and after some time you realize you don't play the game anymore, and haven't been using its alias for a long time. \
This alias indeed used to be quite a useful one, but now it's just trash in your shell configuration; something that you need to scroll through / think through when doing something unrelated. Unclean.

# solution

I created a bookmarking system, where I both “get” and “set” the data interactively, on a hotkey, and therefore don't experience any stoppages. \
The name of each bookmark is simply a *key* that I press, rather than a name I have to type out. \
All of these bookmarks are kept in a json file, so that I can easily back them up, and so the same bookmarks could be reused in multiple different programs for possibly different purposes, and different contexts.

This is how the program [harp](https://github.com/Axlefublr/harp) came to life.

There are multiple concepts that harp the idea (rather than harp the program) brings to the table.

First, I need to be able to store multiple separate sets of bookmarks (a bookmark is called a “harp”): in our beginner example, I want a set of harps which stores file paths, and another set of harps that stores directory paths. \
To accomodate this, harp has “sections”. A section is a namespace, that isolates a set of harps, so that they don't conflict with other sets of harps. \
So I may have a harp section named `harp_files` that will store file paths, and another harp section named `harp_dirs` that will store directory paths.

Next, I mentioned that a given harp (bookmark) is named via me pressing some key. I have a harp `a`, a harp `f`, a harp `ctrl+j`. \
These harp keys / names are called “registers”. \
You can think of vim registers, that you access via `"a` — harp registers are a very similar idea. \
Under each register, we hold some data. \
So, file path harp `a` may store the path `~/fes/dot/colors.css`. Directory harp `alt+k` may store the path `~/fes/lai/bog`{{fn(i=1)}}. \
The data of a register is called a “harp”.

Since registers are simply keys that I press, they can start to collide pretty quick: *this* is why we have sections to separate the sets of registers — I can simultenously have a file harp `a`, and a directory harp `a`, and they won't conflict. \
There is an extra feature I'll talk about later that minimizes collisions even further.

To accomodate this entire idea, I would need to interact with a `HashMap<HashMap<String, Vec<String>>>` in every program that I want to implement harp in. I would also need to interact with the json file that actually holds this data, deserializing it into the program, and then serializing it again. \
This is quite doable in some programs, but completely impossible in others; \
That's where harp *the program* comes into play: it's a single, simple interface to interact with harps.

In most places, I simply use `harp` as an external shell command, to either supply data, or get data from stdout of. \
In other cases where I'm editing the rust source code, I can use `harp` as a library. \
This ends up making it *much* simpler and faster to integrate harp into a given program, as all the boilerplate steps are taken care of for me by `harp`.

# harp actions

Let's start looking into what a harp implementation ends up looking like on a more practical scale, now that we have the main concepts understood. \
I'll be showcasing the harp implementation I have in my [helix fork](https://github.com/Axlefublr/helix), but the ideas are quite flexible and can be implemented into other editors and various programs.

The main idea of how harp is practically useful is via “harp action”s. \
Each harp action has a “set” and a “get” method. \
“set” takes some data from the environment, and stores it. \
“get” retrieves that stored data, and uses it somehow.

## file harps

Let's take the most basic harp action, the one that started it all: file harps. \
File harp set takes the filepath of the current buffer, and stores it. \
File harp get takes the stored path, and *opens* it.

Even just this is flabbergastingly powerful. All of your various offshoot config files that you want to jump to frequently? \
Set file harps for them, and now you can get to them incredibly quickly.

Most of what I do is edit config files… So I use this incredibly often. \
I have file harps bound on <kbd>space+s</kbd>, so to get to all of my most wanted files, I only need to press three keys. \
Helix config — <kbd>space+s+c</kbd> \
Foot config (what a strange combination of words) — <kbd>space+s+f</kbd> \
[Program setups](https://github.com/Axlefublr/dotfiles/blob/main/scripts/setup/dode.fish) — <kbd>space+s+d</kbd>; yada yada etc etc.

Now, how did I *set* all of those harps? \
<kbd>space+S</kbd> is my file harp *set* hotkey! pretty natural, right? :3 \
I get to the file that I want to mark, and press <kbd>space+S+some_key</kbd>. \
Similar to how the get action interactively asks me for a key to press, the set action does too.

So immediately as I realize I want to access the same file over and over again, I can set a harp for it on the fly, and just continue on: I don't need to divert myself into some config file, I just press a key!

{{hr(id="why-config")}}

I strangely specify “config files”; there's nothing about file harps that enforces you to use them only for config files though! \
Why did I choose that wording?

You will set file harps for the files that you visit the most often, as I mentioned. \
When coming up with a harp you'll like using for a file, you go through an interesting process.

Say I want to make a harp for my helix config. \
I access it so often that I don't want to press <kbd>h</kbd> for it, which would be the most obvious key to pick. \
<kbd>c</kbd> feels quite comfortable to press, so I'll go with that.

I have a file where I store various [colors I use often](https://github.com/Axlefublr/dotfiles/blob/main/colors.css). \
The obvious harp key for it is <kbd>c</kbd>, but it's already taken by my helix config! \
I could of course just move the helix config to somewhere else, but I don't want to: it's far more important and common than going to my colors file. \
Next idea is to put the colors file at capital <kbd>C</kbd>, but whoops! It's already taken by my [anki](@/how-to-anki.md) css file. *That* one I could reasonably move as it's not particularly important, but I decide I don't wanna bother right now, \
and arrive at <kbd>ctrl+c</kbd>.

Because of [home row mods](@/erm/index.md), it's actually still quite convenient to press for me, so it's still a good harp key. \
But you can probably see how, as you harp more and more files, you'll end up making bigger and bigger compromises with yourself, coming up with less and less memorable harps, which ends up in you underusing them — you're never quite sure what that one harp was for that one file, because it's <kbd>ctrl+alt+y</kbd> or some shit at this point.

This poor scalability is the reason why I specify “config files” — within this scope, I absolutely have more than enough potential nice harps to use, and don't have to get to the <kbd>ctrl+alt+y</kbd> point (unless it specifically makes sense for some given file). \
But this poor scalability is what will make it unpleasant / unlikely / unreasonable to use harps for temporary-ish files, like files in some project you're working on.

Now this is the part where you'll shit you're pants. I solved this.

I present to you the next harp concept 🗣️✍️🔥:

# relativity

The default harp relativity is “global” — it's not relative to anything and the sections used for the harp actions are also normal: `harp_files`, `harp_dirs`, etc. \
When you set a global harp, you'll be able to get to it no matter where you are. \
This is the default behavior for all harp actions.

But other relativities, aside from “global”, work in a different way. \
They limit the *scope* of your reach depending on some context from your editor.

Let's learn with an example. \
Say I'm working on my [helix fork](https://github.com/Axlefublr/helix), and noticed a file that I want to harp: `helix-term/src/commands.rs` \
The obvious harp key that comes to mind is <kbd>c</kbd>. \
But as you've now seen above, all of <kbd>c</kbd>, <kbd>C</kbd> and even <kbd>ctrl+c</kbd> are already taken; \
And I sure as hell don't want to use <kbd>alt+c</kbd> for this.

The “directory” relativity comes to the rescue! \
Rather than using the global `harp_files` section, the directory relativity creates a new section by using your current working directory. \
I store my helix fork at path `~/fes/ork/hx`, and when I open the editor to work on the editor (lol), \
`~/fes/ork/hx` *also* becomes the current working directory of *helix*. \
**That** is what's used by the directory relativity, and you end up using the section `harp_files_~/fes/ork/hx`. \
Notice how the path to the current working directory became a part of the section name! \
This is how all relativities are implemented :3

What this actually *gives* us, is the following behavior.

I press <kbd>space+S</kbd> to set a new harp for `helix-term/src/commands.rs`, \
and see `harp file set (global)` in my prompt, notifying me that I'm currently in the global relativity (using section `harp_files`). \
Next, I press <kbd>.</kbd> to switch to the directory relativity, \
and notice that in my prompt: `harp file set (directory)` (now using section `harp_files_~/fes/ork/hx`). \
I now press the actual key I want to use for the harp — <kbd>c</kbd>. \
The harp is now set! And it will not conflict with that other harp I have on <kbd>c</kbd> for my helix config, because it's stored in a different section!

Chances are, if I want to go to `helix-term/src/commands.rs`, that means that I already opened the helix fork directory and started helix there. \
So storing the harp for the file *globally* doesn't really make any sense — it's quite a waste! \
Storing it “inside” the only directory from which I'll ever want to access it is far more efficent.

So, to reiterate, now I press <kbd>space+s+c</kbd> to harp into my helix config (from anywhere), \
and press <kbd>space+s+.+c</kbd> to harp into `helix-term/src/commands.rs` (from `~/fes/ork/hx`).

I can create and use project specific harps, resulting in the seemingly restrictive system being quite scaleable!

*All* relativities{{fn(i=2)}} exist for *all* harp actions{{hn(i=3)}}. \
Some relativities don't make logical sense for some harp actions, so I'll introduce the other ones as they come up in usefulness for the other harp actions, as we go.

{{hn(i=2)}} You only know “global” and “directory” so far. \
{{hn(i=3)}} You only know harp files and have *heard* of harp directories so far.

You might find yourself almost always using the directory relativity for file harps; That's a pretty reasonable approach! \
You can change the default relativity *per* harp action. For example, you could set file harps to automatically assume the “directory” relativity. \
Where to access the global relativity again, you'd have to press <kbd>'</kbd> before the harp key, but wouldn't have to press <kbd>.</kbd> every single time. \
As you discover your own harp workflow, you'll find the relativities you use most for each harp action, and will be able to make it more ergonomic and fast 🚀 for yourself :3

## directory harps

Harps don't have to be exclusive to your editor! \
This harp action exists for me both in helix, *and* fish shell!

Zoxide is wonderful, but still requires me to type at least a few characters to get to a specific directory that I want. \
And if I stand my ground and use only one character, it'll likely jump me to the wrong directory; at least, I can never tell ahead of time where I'll end up in, and so it all ends up being a waste of mental cycles.

Instead, I use directory harps! \
“set” takes my current working directory,
“get” `cd`s into it.

My shell, of course, has a current working directory; but did you know your editor (helix / nvim) has its own, separate, current working directory? That's the reason I have directory harps for both!

I do a lot of configuration, so I always need to travel to my dotfiles directory quickly and directly. \
Directory harp `d` lets me jump to `~/fes/dot`. \
My blog, similarly, is something I need to get to often; `~/fes/lai/bog` is on harp `x`. \
The various notes I have that may or may not be temporary are extra important to be instantenous: `~/fes/foe` on `k`. \
I fuck up something in a [magazine](@/magazines.md) — use harp `m` to quickly get to `~/.local/share/magazine` and `git reset` some magazine action.

I used the keys/letters that were easiest for me to remember for each path, so they got into my muscle memory pretty quickly. \
I now consistently save mental effort getting to these directories, and can instead think about anything else instead :> \
I never had to type out any of those paths to *set* them, either! \
I simply get to that directory somehow (maybe by using zoxide, too!), then press <kbd>ctrl+alt+m+some_key</kbd> in my shell. \
My current working directory is automatically passed, so no need to provide it manually!

Then in helix, if I ever realize I need to change directory to one of those paths, I can use the directory harp action *within helix* — I made *shell* directory harps and *helix* directory harps share the same section, so setting a harp in one will set it in the other! Very convenient.

The pain point of `~/.config`, `~/.local/share`, etc, is also completely solved by directory harps! And *you* get to decide what key makes the most sense to you to press to get to them. \
With fuzzying, you get your perfect query enforced on you — with harp, *you* get to name it. \
`~/.local/share` makes you feel like pressing <kbd>z</kbd>? Sure whatever. \
`~/.cache` is <kbd>ctrl+alt+h</kbd>? You go girl.

# I lied

None of the relativities are particularly useful with directory harps! So let's talk about a lie I told you.

My harp file *set* hotkey is not actually <kbd>space+S</kbd>!

Changing relativities is not a one time deal: <kbd>.</kbd> to switch to the directory relativity, <kbd>'</kbd> to switch to the global one, and other relativities you do not yet know, all act as *hotkeys*.

You press those keys to change your current ‘mode’. \
What this means, is that you can press them as many times as you want — you can jump between the relativities, doing like 15 “nevermind”s, before ultimately deciding (or cancelling with <kbd>Escape</kbd>) on the one that you want and pressing the actual harp key. \
<kbd>.'.'.''.'.'..'.'..'</kbd> is something you can press in sequence while using a harp action, and it's completely normal.

It didn't always function this way, and I am really satisfied with my recent harp refactor that made this behavior possible. \
I really enjoy not being rushed to pick, and instead be able to stay 🤔ing for a while until I decide.

My previous set hotkey *was* <kbd>space+S</kbd> at the time, and I quite disliked how I needed to think *ahead of time* (which is quite [unhelix-like](@/why-even-helix.md)) whether I wanted to set or to get. \
Then also, having to hold shift for the set variants made a certain upcoming harp action kind of not worth it to use, to its fullest extent. \
And that's why I made the change:

Pressing <kbd>Space</kbd> while in a harp action toggles between the `get` variant, and the `set` variant. \
Instead of pressing your “harp file get” hotkey to open a file, and your “harp file set” hotkey to harp it, you *simply* use your “harp file” mapping, and *then* decide. \
You start off in the `get` mode, but can press <kbd>Space</kbd> to go into the set mode. \
Then if you realized you didn't want to set, you can simply press <kbd>Space</kbd> again to go back to get. \
And you can do this infinitely!

## relative file harps

As I started actively using harp, I noticed an interesting pattern. \
Project structures are often not that unique!

In a rust project, there will always be `Cargo.toml`, one or both of `src/main.rs` / `src/lib.rs`. Possibly `.rustfmt.toml`. \
In lua, `.luarc.json` might be something you want to access every so often. \
Basically every project will have a `README.md`, maybe a `CONTRIBUTING.md`; some of my projects have a `RELEASE.md` that is automatically attached to the github release.

With harp, you might find yourself *set*ting the same relative paths over and over again, per each project. \
And the file first needs to exist and be open, for you to *set* it, so that's a lil extra effort as well. \
Seems like a bit of a waste, no?

This is where relative file harps come in.

When normal file harps store the *full* path, relative file harps only store the *relative* path of the buffer, from your current working directory.

So say your cwd is `~/fes/dot` and you have `~/fes/dot/project.txt` open. \
If you set a normal file harp for it, the entire `~/fes/dot/project.txt` would be stored, and you would only be able to travel to `~/fes/dot`'s `project.txt` file. \
But if you set a *relative* file harp, only `project.txt` is stored. \
And now, whenever you *get* the harp, you will open the `project.txt` file of the *current* project, rather than some exact file.

What this lets you do is access files that do not even exist yet! \
Rather than having to first create the file somehow, you immediately jump to where it would be, and on your next write it will automatically get created. \
So we're solving two problems here:
1. having to set harps for the same files for every project is laborious
2. having to first create the files to then make harps for is laborious also

To reiterate: use relative file harps when you might want to get to different *actual* files, but that are of the same *structure*. \
Use normal file harps when you want to get to *exact* files.

Here are my relative harps:

|Register|Filepath|
|-|-|
|<kbd>r</kbd>|README.md|
|<kbd>e</kbd>|RELEASE.md|
|<kbd>k</kbd>|[project.txt](@/magazines.md)|
|<kbd>g</kbd>|.gitignore|
|<kbd>j</kbd>|justfile|
|<kbd>l</kbd>|lib.rs|
|<kbd>c</kbd>|Cargo.toml|
|<kbd>m</kbd>|main.rs|

I underuse relative harps honestly; I should harp some more 🗣️

## register harps

If you're a vim / helix / kakoune user, you might have noticed how using harps is kinda similar to using vim registers. \
With vim registers, you access a “key”, and then get text that you can interact with in some way; usually by pasting it.

In helix, the contents of your registers do not stay across sessions, making them fairly useless. \
And that's why register *harps* is a very useful idea!

As I (hopefully) mentioned already, *set*ting a harp makes it *instantly* available in *all* sessions, \
so you don't have to worry about helix “forgetting” the contents of your registers after you exit it.

To *set* a register harp, first simply copy (yank) something to your `default-yank-register` \
(`"` by default (meaning that you simply copy normally, no special magic required)). \
This is the “context” that this harp action uses for the set action: it takes the contents in the `default-yank-register`.

Later on when you *get* that register, it places the stored contents back into your `default-yank-register`, letting you paste them normally.

I'm gitty of the power of this harp action, and explaining it to you!! \
But first, let's introduce *another* relativity :o

## filetype relativity

You can make a harp relative to the current buffer's filetype. \
It uses the filetype that helix sets for the buffer, rather than just taking the buffer's file extension! \
So if you use the filetype relativity on a markdown file, `!markdown` will be appended to the section name, rather than `!.md` that you may have expected. \
The filetype relativity even works for files that don't *have* a “real” filetype! \
When the buffer doesn't resolve to any real filetype, its filetype is set to “text”. \
So, if you use the filetype relativity in an `example.txt` file, `!text` will be appended to the section name.

To switch to the filetype relativity, press <kbd>;</kbd> while accessing a harp action.

## back to register harps

The filetype relativity + register harps lets us create a simple snippet system!

For example, [I despise lua](@/why-I-hate-lua.md), and don't like how long it takes to type in the syntax for a function object / closure, and I also happen to dislike the default snippets that the lua language server provides for them.

I could yank the following:

```lua
function()
  →
end
```

(the `→` there is a tab)

And put it into a filetype-relative register harp (just make sure you're in a lua file while you do this). \
Now I can “summon” it whenever I want, by pressing `<space>w;f` + the paste key{{fn(i=4)}}.

Pressing 4 keys to get to it *may* be not worth it for you for the specific example, but you can probably see how a bunch of various language-specific boilerplate you don't like typing out can instead be harped, consistently saving you effort :3 \
In my personal usage, I find myself using the filetype relativity the most often with register harps, so I set it as my default relativity for them. \
In other words, getting that function object syntax example is just `<space>wf` for me.

Also, I use register harps really frequently for shebang lines! \
For example:
```sh
#!/usr/bin/env fish
```
I store this in register harp <kbd>#</kbd>, relative to the `fish` language. Then I store the following relative to `nushell`:
```sh
#!/usr/bin/env -S nu -n --no-std-lib
```
Once again, *also* in harp <kbd>#</kbd>.

So you can create a semantic for yourself that makes it easy to remember the *intent* behind a harp, yet store many different ones thanks the various relativities. \
Here, it makes sense for me that a shebang line starts with a <kbd>#</kbd>, and so I use that key as a neat mnemonic. \
Making use of easy to remember mnemonics / going with what feels the most natural is a great way to make a good use of harp.

{{hr(id="another-alternative")}}

Another good default relativity you could use instead of global, is the directory relativity. \
Then, by default, you will simply have vim-like registers that are scoped to a project, so you can use them a bit more willy-nillily than global ones or filetype-relative ones, and simply temporarily switch your relativity when you *do* want those.

No need to worry about your registers disappearing, and no need to worry what else you saved for later while working on a completely different project.

## search harps

Whenever you search for something, the search pattern you used is stored in the `/` register. \
Interestingly, this works not only for the `search` action (<kbd>/</kbd>), but also for the “ripgrep project” action (`global_search`).

When you *set* a search harp, the regex pattern stored in your `/` register is stored. \
When you *get* a search harp, it is taken from the storage and placed back into your `/` register.

The way that searching and the `/` register works in helix specifically makes this a very nice to use and flexible functionality. \
You see, `/` doesn't store “your *last* search”, it stores “your search”. \
So when you change the contents of your `/` register, you change what you will be searching for when you press <kbd>n</kbd>/<kbd>N</kbd>. \
There's no need for the harp action to automatically trigger a “and now you jump to the match!” action, *you* get to decide how you proceed. \
So you can get a search harp, and then decide to jump backwards with <kbd>N</kbd>, or forwards with <kbd>n</kbd> — harp doesn't make the decision for you because helix's design doesn't require it to, muah!

But another really powerful thing that filling the `/` register does for you, comes back to `global_search` once again. \
If you open `global_search` and don't type in any pattern yet, it shows the pattern stored in the `/` register, greyed out. \
Now, if you press <kbd>Enter</kbd>, it is automatically accepted as your input.

In other words, you could get a search harp, then invoke `global_search` and simply press enter, to search for a stored query!

This capability is not unique to `global_search` though, as if you press `/` to do a buffer search, you will *also* see the same greyed out search pattern that you can accept with <kbd>Enter</kbd>. \
Ooooor, you can press <kbd>Up</kbd> to actually paste in that pattern, for you to *edit*.

The way that searching and the search register works in helix allows it to be very flexible, as you can see; \
It is thanks to this that register harps is the first harp action that makes great use of *every* relativity! \
Let's go through them.

### directory

The `global_search` capability lets us have lsp-like capabilities without relying on an lsp! \
First you notice what things you keep searching for in the project, and interactively try to match them with regex by using <kbd>/</kbd>.

You can take your time doing so, trying out different approaches until you confirm it works by simply <kbd>n</kbd>ing to it. \
Then, you set a harp for that, using the directory relativity.

Now, whenever you want to search for the query again, you *get* this search harp you created, open `global_search` and press enter. \
Especially if the directory relativity is your default one for search harps, this is not too bad!

We created a basic snippet system with register harps, and now we can use *search* harps to create a basic textobject system!

❗introduce the last, buffer relativity

## mark harps
## command harps

❗talk about how the relativities affed each of the harp actions, including the useless ones

# footnotes

{{hn(i=1)}} To your possible surprise, yes this is a [real file path](@/optimizing-paths/index.md) I have. It's for this blog!

{{hn(i=4)}} Which is <kbd>p</kbd> for you, but <kbd>o</kbd> [for me](@/stray-from-defaults/index.md). \
Also, you don't necessarily have to *paste* it. You can replace your selection with it, for example! \
So register harps simply blammo the text into your `default-yank-register`, and then you can use it how you normally would — no specialcasey usage required!
