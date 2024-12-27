+++
title = 'the perfect scripting language... is rust?'
date = '2030-12-31'
draft = true
+++

In practice, there are about two languages that I would *want* to use for scripting. \
My absolute favorite programming language: fish shell, and my second favorite programming language: rust.

> Yes, I'm not joking! Fish shell is my favorite programming language!

I write a lot of fish shell and enjoy it, but there are *some* cases where it's not enough. Anywhere I need a hashmap, for example, I'm quite fucked. However, using rust for scripting is very bothersome.

I like to say "in rust, hard things are easy and easy things are hard" — it just so happens that most scripting is the easy things, so I naturally "fuck no"ed out of using rust for scripting purposes.

Plus, a compiled language that kinda needs its own directory to function well, feels laborious to pick for a simple script. \
Starting a project (coming up with the name, doing initial setup of Cargo.toml and such) is a big part of that 😩ness.

I'm in an annoying spot: fish shell and rust are on the opposite sides of the spectrum of "flaky but fun" vs "solid but unfun". \
And so I decided to go look for a new programming language to learn!

{{ hr(id="shit-languages") }}

I had the feeling that that are a plethora of good programming languages out there, and I don't hear about them simply because I wasn't looking for a language to learn. "Surely I'm just ignorant". \
Turns out, there are not that many reasonable languages to begin with, and that gets worse when you try to go for a *scripting* language.

# nushell

I hear very good things about, and it seems genuinely great, *however*!

I tend to only learn things that can benefit me in some way. It is *very* hard to learn something if I see no benefit in it. \
For example, I quit learning SQL 2 or 3 times, because of just how strongly I find it useless personally. \
I don't work at some corporation that needs a database, and all of my programs are *designed* to use simple structures, like json. \
Silly to overcomplicate something when there's no need, isn't it?

Because of how I work, I like to come up with, and exaggerate problems, to trick myself into learning more than I would otherwise.

This blog is an amazing example of this. \
Previously, all of my blog posts were github gists. That worked very well, but is kinda wack once you start to *continue* writing stuff: no clear ability to RSS, no "central page". \
To make myself a blog website, all I had to do is install hugo and pick a theme. That would solve the actual problem. But that's no fun!

Instead, I decided to go with zola, starting from a pretty much empty template, and wrote all the html and css for the blog website you're currently on. \
It took me a lot of time and effort getting things right, and that is my **first** experience of writing a website!

I took a problem, and made it bigger to give myself an opportunity to learn something that will be useful *elsewhere*.

I'm saying all of this to point out: with this approach, learning nushell would be a waste. \
I already know and use a shell language, having *another one* would *just* solve my problem, and probably wouldn't let me learn something extra on top.

# python

I don't like it

# perl

Be serious

# php

I already said be serious. \
"Ah yes let me learn php for scripting" — statements dreamt up by the utterly deranged

# lua

[refer to this](@/why-I-hate-lua.md)

# ruby

Is an interesting case. \
The requirements I had for my ✨language of dreams✨ are these, from most important to least:

1. In the middle between fish shell and rust on the fun vs unfun scale
2. Fast startup time
3. I would feel cool saying I use this language
4. Not worse than python
5. Interpreted

#2 is a bit imperfect, but good enough. the **#3** is the real issue.

"Ah yes, I'm learning ruby in 2024" is not really something I want to say. Ruby is an old and decrepit language, and I would rather go learn something else.

{{ hr(id="compiled") }}

I kinda ran out of options! I thought there would be wayyyyy more choices for interpreted programming languages, but I guess not?

The big annoyance with compiled languages is that they need their own directory for their own bullshit, you can't just directly run the source file as a script. Even if you can, you are probably trading startup speed / performance / both.

And this is where I discovered a neat little program called [scriptisto](https://github.com/igor-petruk/scriptisto)!

Scriptisto lets you use a hashbang to be able to run compiled languages as scripts: it handles all the directory bullshit, as well as storing compiled binaries for you.

So, after the first time you run a "script", it doesn't need to recompile and so the startup is instant! \
Or rather, the startup is as fast as it normally would be for a compiled binary of the language.

Having this in my toolbelt gave me some new options! Surely if I include all the compiled languages, I'll find something cool :D

# crystal

Seemed like the perfect contender. It's heavily inspired by ruby, but unlike it, it's not dying! \
It's not used that much, so that gives me extra cool points. \
"Crystal" is also a cool name, and with all that I would absolutely feel cool saying I use this language! \
#2 solved 😌

I decided to pick crystal as the language to invest my time into. I joined the discord server for it, and got really excited about it.

After a couple of days of my honeymoon phase, it hit me: *this language doesn't have a working treesitter grammar*.

I tried to blow it off at first, saying "Worst case scenario, I'll just disable syntax highlighting". \
But after reconsidering, syntax highlighting *is* important to me. I'll argue far more than a working LSP.

> I write a lot of fish shell code without an lsp \
> A fish shell lsp [exists](https://github.com/ndonfris/fish-lsp), but I didn't find it useful enough to keep

Sure, you can use the ruby treesitter, but having syntax highlighting be potentially *really wrong* is much worse than not having it at all.

And yeah, the [lsp](https://github.com/elbywan/crystalline) is made by some burnt out guy, and isn't very good or functional. \
Fair enough on the guy, but if this is the best we have (outside of vscode), it *is* a pretty sad state for a language.

I really liked crystal though, so I intend to come back to it once it gets a working treesitter grammar.

# nim

At first I got really excited about this language! I thought it was the one 😌

But the more I learnt it, the more I felt "isn't this just a worse python?" \
And so I quit.

# ocaml

Ocaml is probably the peak of worth for my approach of learning far more than expected, when solving a problem. \
The closest to a functional language I've used is rust, and calling rust straight up a functional language is a big stretch.

So, I get to learn a real functional language that I can feel really satisfied about!

Ocaml also has some other great benefits: the compilation speeds are INSANELY fast! \
You can almost just go for always recompiling on the fly, without that big of a loss.

I went ahead to learn the language, and let me tell you: GOD is it difficult.

Coming from having never tried a functional language, the way it bends your expectations is really difficult to process — it's as if you're learning programming for the first time.

Did you know that in functional land, you're apparently supposed to (always?) use recursion instead of loops. \
That seems completely insane to me ngl, but of course I'm giving it the benefit of the doubt and think "I'm sure there's a reason for this".

I never reached that reason, though. \
Ocaml is way too fucking difficult, I read a screen's worth of tutorial and I'm burnt out for the day. \
There's no way I'll learn this language fast enough to actually practically use it, so I put it in the backburner — a language that's useful to learn entirely for the learning part, not for objective benefit.

# go

Is a language that's been hyped a lot, and I never got why. \
While going through this adventure, I finally understood — there are not many good programming languages, and any time a half-decent one comes out, it naturally ends up being exciting.

Same reason why rust popped off. \
Rust has a lot of issues, but it just so happens to be the least bad out of basically all programming languages.
My experience with go was very short: I chatgptied something along the lines of "how do I for loop through the lines of stdin", looked at how ugly the code for that is, and completely disregarded the language.

I was looking for something in the middle of fish shell and rust, remember? Go felt very very close to rust. \
And if that's the case, I might as well just use rust.

{{ hr(id="ran-out") }}

Now after going through all the possibly viable *compiled* languages, I'm out of options again :c

What, am I stuck with no good language to learn? At this point actually fuck the side benefit, I *do* need a language to solve higher-end scripting than fish shell.

And so I decided to eat sand and learn a similarly sandy (old) language: ruby. \
I didn't have much expectations going in, I just knew that some people like it quite a bit.

Oh boy! Wow! What a language! \
As I started reading the book, slowly I started to fall in love with the language. The way this language is constructed is both very elegant, and really wack — **exactly** the experience I was looking for! Matter of fact, even better than I expected!

I was worrying it would have too little functional programming parts, but nope! Ruby got my back on that too. To the point that that functional part of the language felt easier to approach than rust's one is!

---

I wandered across quite a few languages, coming from a lot of pain from rust. Finally, I decided to try it again.

I go on to start writing a script, and notice a few things I powered through before:

## inlay hints

Even for a simple rust program, the inlay hints get **intense** quick. \
They *are* very helpful, but only *sometimes*: generally I already know what types things are, or it doesn't matter.
In that case, inlay hints just end up being visual clutter.

I decide to disable them by default, and give myself a nice mapping to toggle them for when I need them.

## diagnostics

One missing piece of syntax, and my screen is yelling at me in sheer red. \
Often times I am literally in the middle of fixing what I already know to be incorrect. But the brain works in interesting ways — even though I know what I'm doing and am succeeding, the red all over my screen subconsciously tells me "you are failing".

Let's disable diagnostics as well, and opt into them for when I'm not sure what's happening.
