+++
title = 'harp'
date = '2025-06-02'
draft = true
+++

Travelling to files in text editors is very slow at best.

There are three main ways to travel to a file from your text editor. Let's discuss them.

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

❗this sucks because of downloads and documents
❗you're led to optimize your filepaths, which makes them look stupider

❗perfect query requirement again, but now even stronger
