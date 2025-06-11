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

Let's start looking into what a harp implementation ends up looking like on a more practical scale, now that we have the main concepts understood.

The main idea of how harp is practically useful is via “harp action”s. \
Each harp action has a “set” and a “get” method. \
“set” takes some data from the environment, and stores it. \
“get” retrieves that stored data, and uses it somehow.

Let's take the most basic harp action, the one that started it all: file harps. \
File harp set takes the filepath of the current buffer, and stores it. \
File harp get takes the stored path, and *opens* it.

Even just this is flabbergastingly powerful. All of your various offshoot config files that you want to jump to frequently? \
Set file harps for them, and now you can jump to them incredibly quickly.

❗but you'll run out of them quick, no?

# the power

❗relativity

# footnotes

{{hn(i=1)}} To your possible surprise, yes this is a [real file path](@/optimizing-paths/index.md) I have. It's for this blog!
