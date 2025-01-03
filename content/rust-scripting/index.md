+++
title = 'rust for... scripting?'
date = '2025-01-04'
+++

If the title of this blog post is a genuine question you have, and you want to just skip to the most reasonable solution: <spoiler>use cargo-script</spoiler>

# why

I recently realized that the two languages I like using, fish shell and rust, are ridiculously far apart. \
I wanted to find myself a language I could use for scripting that is non-viable to do in fish shell, but too annoying in rust.

Turns out, there are not really any languages that fit all of my criteria!

Python, Ruby, Crystal, Ocaml, Nim, Julia, Go, Nushell — these are the languages I tried, or considered going for.

I actually wrote the majority of the blog post on my adventure looking for a good programming language, but found what I wrote kinda boring and soulless, so I won't get into why each of those languages didn't fit me. \
Once I'm "over" something, it becomes incredibly boring for me to talk about it, and so I missed the train of writing a blog post on that, lol.

The TLDR is that I came back to rust, reinvigorated to use it more!

# on rust

Rust has always felt really *solid* to me. But like a cock, it is sometimes too hard to work with.

After trying out so many other programming languages, and spending the most time and effort on Ruby, I re-started to appreciate that solidity of rust, and changed my approach to the language a bit.

Inlay hints turned out to be visual clutter I came to accept. Removing them by default, and only toggling them when I *need* them, has helped immensely to look at rust more *calmly*.

Diagnostics in rust are very helpful, but sometimes overwhelming. Similarly, once I see a bit too many diagnostics, I disable them. Once I need the extra advice, I enable them back.

I was constantly *fighting* rust, getting annoyed at how I'm meant to do seemingly simple things. \
Getting the modification time of a file is kinda cumbersome in terms of the syntax, \
working with files is often way too involved, calling external commands is boilerplaty; especially so once you literally just need the stdout as a string.

Often times it felt like rust overcomplicates things. For good reason, mind you, I just happen to not care. \
I'm a very selfish programmer, I don't need cases I won't come across to need to be solved.

But, it is the language that rust is, and some code *will* look boilerplaty / ugly. \
I've come to *accept* that, instead of being annoyed that my code isn't syntactically perfect. \
Now I'm more okay with some parts being kinda hard to look at — I will simply skill solution my way into being used to it.

And lastly, rust has always felt like *responsibility*.

Making plugins for neovim in lua had the nice benefit of that I *didn't have to*. \
I could just have some code in my own config for a couple of months, completely working. \
And then, once I'm ready to, I could package it all nice and prettily.

Fish is even more amazing than that, because you can have a lot of callable executables in single files, thanks to a technique I use and will probably talk about in the future. So, the pressure of writing something new in fish is also really really small.

Starting up a new CSS extension with stylus is *slightly* a process, but it's straightforward and fast enough where it's not that big of a commitment. If I want to, I can easily shrink the ridiculously large sidebar with css for some stupid website, without it breaking my flow particularly much.

Rust is in a completely different league. \
I have always wanted to *avoid* using rust because of the sheer pressure I put on myself.

First, I need to come up with a name for a project, and create a new repository and directory for it. \
Nope, you already lost me!

Coming up with a project name and being happy with it are really fucking difficult, and are not something I want to do for a on-a-whim script that probably only I will use.

Then, setting up rustfmt config, github action for auto-making releases, keeping track of the version and the versioning tags, remembering to push them, writing DOCUMENTATION in both the readme and source code with `///`, picking apart which information should be in which side of the documentation...

IT'S TOO MUCH!

# but not anymore

I created myself a new system that allows me to use rust for scripting in a pretty fast and straightforward way.
No more various bullshit to take care of for the 0 users I'll have — sheer selfish programming!

Let's start where I started, to figure out why I ended up creating my own system.

[scriptisto](https://github.com/igor-petruk/scriptisto) is a really neat program that I used for the various languages I have tried. It allows you to use a compiled language for scripting, conveniently!

You can run that script just like it's written in an interpreted language, because scriptisto can be used as a shebang.

So, something like `./script.rs` is possible. The first time you run that script, scriptisto compiles it. On the following runs, it simply resolves to the compiled binary and runs that, without needing to recompile.

I've had an amazing experience with scriptisto, and would heavily recommend it. However, it was strangely slow with rust!

After the first run, the following runs *still* had awful startup time. Around 3 times slower than ruby! And ruby has a pretty shit startup time already.

When you can tell the startup speed by an eye test, that means it's *really* bad. So, I decided to try using [cargo-script](https://github.com/DanielKeep/cargo-script) instead, and it indeed didn't have that issue! The following runs were definitely fast!

{{ hr(id="definitely-not-foreshadowing") }}

*Until they weren't...*

One of the first programs I wrote is called [`velvidek.rs`](https://github.com/Axlefublr/dotfiles/blob/main/scripts/velvidek.rs). \
I don't use the number row; instead, right meta key + fdsrewvcxa are my numbers (1234567890). \
Velvidek takes those letters, and replaces them with their appropriate digit. Using this program, I can shim in support for my number layer, without requiring me to press the right meta key. \
Now instead of having to do `fg %2`, I can just do `f d`, thanks to the fish function I made that made use of velvidek. Very helpful and important addition to my [suspend-based workflow](@/suspend/index.md).

I was pressing `f` to go back to the suspended process, and noticed the operation be slower than I expected. \
I use fish shell's [`time`](https://fishshell.com/docs/current/cmds/time.html) to check how long it takes for velvidek to execute, and see ~70ms.

Well holy shit, that is entirely too slow! For a program as modular, small, and fast as this, this is ridiculously long.

I was running velvidek as a script through cargo-script, so, using the shebang `#!/usr/bin/env run-cargo-script`. I decided to check how long `env` takes to figure out the path to the binary. 3ms. So, env is not the problem here.

It turns out that cargo-script is wasting a lot of time doing *something*. Well this is concerning.

But interestingly, a fish *script* takes around the same time! ~80ms for a roughly equivalent functionality script I tested. So, at least cargo-script is faster, or on par with a shebanged script in a different language; that's good news.

The bad news is that it *is* too slow regardless. When you write a fish *function*, the time it takes to execute it is in the nanoseconds. When you turn it into a script, it gains these extra ~80ms of startup time, apparently, and so does a shebanged rust script.

cargo-script caches compiled binaries as I mentioned, so I decide to time how long it takes to execute, if I call the compiled binary directly. 3ms. \
It seems like it takes around 3ms to start a process on my computer, considering that both `env` and a random rust binary take around the same time.

So, in other words, the binary itself is not slow; cargo-script is simply wasting time.

My guess was the following: cargo-script tries too hard to not recompile, massively slowing down the general case.
So, if I make that process stupider, I'll gain a faster general case, while probably having to recompile unecessarily more often.
A tradeoff I'm absolutely willing to make.

My idea is to compare the modification times of the script and the compiled binary.
If the binary is older than the script, let cargo-script continue like normal, to recompile it. \
If it is not older, run the binary directly, and forgo asking cargo-script for its opinion.

This sounds like such an obvious solution, that it's weird they are apparently not using it. \
When you feel smarter than a developer who's more impressive than you, you are *probably* wrong, and that's what I assumed as well.

I'm trying my best to not reinvent the wheel though, and this pre-step possibly working would allow me to continue using most of the functionality of cargo-script, without having to recreate it for myself, to allow for optimization.

Alright, first run! \
The binary is older than the script, and the execution gets forwarded to cargo-script. Fair enough. \
And right about now, it should recompile. Right? Right???

It does not. And so my pre-step just infinitely transfers control to cargo-script, infinitely having that ~70ms startup time.

Worse yet, the time difference between the binary and the script is ~5 seconds. Not *milliseconds*. So, at which point do I consider the binary "old enough" or not?

Going in this direction seems fruitless. And I forgot to mention something: when using cargo-script, rust-analyzer does not help you. It requires there to be a Cargo.toml to give you lsp capabilities; if you don't have it, it gives up and does nothing.

So, for a good developer experience, you kinda need to have a directory where you write the scripts to begin with, and then something that would compile that source code into a cargo-script compatible single-file script.

Considering that I *already* had a "workspace" directory, I thought "fuck it" and went ahead to fully forgo using cargo-script, in favor of making my own system. It will be less convenient, but far faster to start up.

# my system

A "workspace" directory named `wks`. I use this place to write the scripts for the first time. \
There's `src/main.rs` and `Cargo.toml`. Just what is needed to make rust-analyzer work.

Once I finish writing a rust script, it's now time to export it. \
I have a (rust) script that takes main.rs and Cargo.toml, and joins them into a single file. \
The path to the target file I want to write to, I specify as the first line in main.rs, in a comment. \
[Primary link](https://github.com/Axlefublr/dotfiles/blob/main/scripts/scriptister/export-rust-script.rs), [backup link](https://github.com/Axlefublr/dotfiles/blob/777ec42abaaa0313bf89e6bae0810de058e84f6c/scripts/scriptister/export-rust-script.rs).

While writing this export step, I started thinking about how I'd make a script executable. I would probably write another script, that would act as the program in the shebang. But I found something more elegant!

What I want, effectively, is to:
1. not pollute where the scripts are, with their compiled binaries
2. to execute a script, you simply use the script name (for example, `velvidek.rs`)

To achieve that, I can make use of `$PATH`! Once I compile a script, I will rename the resulting binary to the name of a the file, and put that binary somewhere on the `PATH`. This way, I'm typing in a name of a script to execute, but sneakily I'm actually executing the compiled binary stored somewhere else.

This is where the second script comes in. This one is in fish though: [primary link](https://github.com/Axlefublr/dotfiles/blob/main/scripts/scriptister/compile-rust-script.fish), [backup link](https://github.com/Axlefublr/dotfiles/blob/777ec42abaaa0313bf89e6bae0810de058e84f6c/scripts/scriptister/compile-rust-script.fish).

While the export script just writes to the target script file, *this* is the compilation script. It compiles the project, and copies the resulting binary somewhere on the PATH.

While one ensures my work is *backed up* (as a single file "script" in my dotfiles repository), the other one actually makes sure it's executable, by providing a binary with the exact same name, that I can call.

A very sneaky and elegant solution, I'd say! :3<

In the future, if I need to *edit* some rust script, that's where the *import* script comes into play. \
[Primary link](https://github.com/Axlefublr/dotfiles/blob/main/scripts/scriptister/import-rust-script.rs), [backup link](https://github.com/Axlefublr/dotfiles/blob/777ec42abaaa0313bf89e6bae0810de058e84f6c/scripts/scriptister/import-rust-script.rs).

It takes a path to a rust script that I exported into previously, splits it into main.rs and Cargo.toml, and writes those files in the workspace directory `wks`. After I import, I'm ready to edit the script in my workspace, to then export it back again.

Thanks to my [helix fork](https://github.com/Axlefublr/helix)'s command harps, running all of these scripts is really easy, and this has become a way nicer workflow than I expected!

Now that rust is so fast to get running, I'm way more excited to write it, and I already am rewriting some things as rust scripts, when they were written in fish before. It's super nice!
