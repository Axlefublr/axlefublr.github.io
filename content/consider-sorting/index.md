+++
title = 'consider sorting'
date = '2025-05-01'
+++

I'm almost certain you have a problem that you either don't realize, or don't have a good solution for.

Sometimes in code, you need to place many things in some sort of order. And sometimes, there's no one order that is logically significant. \
An example of this is configuration options.

There isn't really an "order" to them, within each section: you are setting *all* of them.

An even more obvious example I have on hand, is a function holding fish script I have, that I use to organize and back up setup processes for various programs: [look here](https://github.com/Axlefublr/dotfiles/blob/main/scripts/setup/dode.fish).

There too, there isn't some particular order to them. Sure, in the initial setup I'll make fish work, then git, then niri, then probably waybar; but aside from those "blocking" setups, that basically act like dependencies for other setups, there is no obvious order — most things simply behave like a set, rather than an array.

Something that I started doing some time ago, is explicitly *sorting* things like this.

Now, looking through this set is organized; it will always be obvious where a given thing is, and additionally it will always be obvious where to insert a new thing, without making the file more messy.

I memorized the indexes of the letters of the alphabet using [anki](@/how-to-anki.md), to make this type of manual sorting way easier for me. No need to sing the alphabet song every time. \
But that's just for the manual sorting I do, like in the setup function holder above; usually things that you may consider sorting are single lines, that you can easily do if you use a half decent text editor.

However even with that functionality, it can still feel a bit too manual to feel worth it, not gonna lie.

This is why I wrote a neat lil program called [partialsort.rs](https://github.com/Axlefublr/dotfiles/blob/main/scripts/partialsort.rs)!

> This isn't exactly a promo, as the program is written as a [wks script](@/rust-scripting/index.md) rather than a normal repo-ed rust program that you could install more easily

The idea of it, is that I can mark sections of files with `[[sort on]]` and `[[sort off]]`. When I run the formatter over the file, the lines between these two markers get sorted!

<!-- [[sort off]] -->

My [helix fork](https://github.com/Axlefublr/helix) then has an integration that allows you to run a global formatter (that can happen to be partialsort.rs) on write. If you use something like neovim, adding a global formatter should, interestingly, be both more *and* less painful. \
Another option I used to use, is to run a formatter like that on git commit hook.

Regardless of how you may simplify the process of sorting things for you, you will soon find out how much cleaner things become. \
Especially for various configuration files, which is my main usecase. After writing the config file for the first time, you are likely to be coming back to it again and again, changing, removing, and adding stuff. \
It gets so much easier and *faster* to do so, when the items you intend to change are set in an obvious order! :D

Consider sorting!
