+++
title = 'why even helix?'
date = 2024-10-21
+++

I've been using neovim for around two years. \
I've rewritten my config three times: for the first time, when I switched to astronvim, and when I switched back from astronvim. At the most recent point, my config was 5000 lines long. \
I've gotten into the habit of reading nvim help pages in their entirety, for fun. \
I have also written [s](https://github.com/Axlefublr/edister.nvim)[e](https://github.com/Axlefublr/harp-nvim)[v](https://github.com/Axlefublr/qfetter.nvim)[e](https://github.com/Axlefublr/lupa.nvim)[n](https://github.com/Axlefublr/selabel.nvim)[!](https://github.com/Axlefublr/dress.nvim)[!](https://github.com/Axlefublr/wife.nvim) plugins. \
And usually spent multiple hours every day configuring nvim.

I'm glazing myself here to point out: I have obviously given nvim a good try; matter of fact the try was unreasonably good! \
And the reason why I switched off nvim is not due to the lack of knowledge about nvim.

{{ hr(id="brighter-future") }}

Considering my [two](@/why-I-hate-lua.md) [other](@/why-I-hate-kitty/index.md) blog posts, this one should be called "why I hate nvim", but I don't actually hate it! \
I just realize that helix is simply the better option.

I've been wanting a brighter future than nvim, for a long time. And while I did look at helix, there was something that stopped me from considering actually switching to it. \
I'm sure you've heard of this stance, or even maybe have it yourself: "helix has no plugins, so I'll just wait for it to get them, and then I'll switch".

Fair enough! That was my idea too! \
However, I'm autistic, so switching to things is a whole ordeal, usually taking me multiple consecutive days, and taking a lot of time, effort, and energy. \
Doesn't really make sense to wait for the plugin system, to then overwhelm myself all at once, does it? \
So I decided to do it the smart way.

# the plan

First, I go on and fully discover helix. Learn the editing model, the bindings; configure it to be fairly usable, go through the entire [documentation](https://docs.helix-editor.com/master) to not miss anyting. \
With *this* basis, I effectively split the overwhelm into two parts, making it more manageable. \
So that when the plugin system comes, I can *actually* switch. \
Yes! My idea was to stick with nvim still, but think about the future in this way.

I failed spectacularly... Or did helix succeed that much? \
Let me elaborate on why I switched to helix so soon, despite intending not to!

# the editing model

Helix's "selection->action" model is ultimately the biggest reason for why I switched. \
Despite being so used to vim's "action->selection" model, it took me 1-3 days to get used to helix's one. \
I believe it's because it feels *that* natural!

The way my brain works is something like this:
1. "uhhhh right so this piece of text"
2. "uhhhhh facken let's do this"

rather than:
1. "uhhhhh soooo let's do this ig"
2. "with uhhhhh this text"

{{ hr(id="pausing") }}

It's fairly often that I know *subconsciously* what I wanna do, and actively "discover" what that something actually is. \
Because of that, I may take ridiculously long pauses between my keystrokes to think.

Nvim has this incredibly annoying default, where your action+selection key sequences are quick time events. I'm serious! \
You have to execute the whole action in some amount of time after pressing the "operator" — otherwise it will time out.

You can of course disable this (via `:h 'timeoutlen'`), but this is just the start of the pattern of bad defaults in nvim. For now I will just state that since this is a default, other nvim functionality (as well as plugins), will often be designed having this behavior in mind. In short, nvim penalizes pauses.

{{ hr(id="an-even-better-point") }}

Coming back to the example above, nvim makes you decide on an action first, and the selection second; right after you just decided on an action, not encouraging pauses, it makes you decide the selection too.

"That's kinda rough when you put it that way, but it's not that bad, right?" — I'll make my argument less strawmanny and still showcase how it's actually really terrible.

Let's do this very very common thing in nvim and fix a default: we will disable the key timeout. \
We'll claim that "well yes OBVIOUSLY you change that" and transform an individual named Bob into the brother of our parent.

We still have a pretty glaring issue with the editing model as a whole, even with key timeout not being an issue.

Whether or not we'll even be able to make the selection is not a guarantee! \
"First you select an action to do and then the area of text it acts on" is not the full story at all.

## vim's *actual* editing model

Especially without plugins to *help* (notice, not *solve*), you will often times **not** be able to select the text area that you *actually* want. \
The more realistic model of vim is:
1. Place your cursor in a position from which you'll be able to make the selection motion later
2. Decide on what you want to do
3. Think about the selection *again*, now actually making it

It's a lie to say that vim has an action->selection model, because you first need to figure out if your selection will even work! \
And because of things like dot repeat, and efficiency (generally doing an action via action->selection is less keystrokes than the visual mode (selection->action) equivalent, in nvim), actively picking to always use visual mode first is a known anti-pattern! \
Think of any vim tutorial: things like "you should use `ciw` instead of `viwc` 🤓" are always (validly) pointed out.

# core philosophy as a solution

In helix, half the time you don't even need to think about how to make the selection, because you already made it just by naturally moving around.

The `w`, `e`, `b` word motions that you already know and love now also *select* the word. This is useful because in helix, you *always* operate on a selection. Matter of fact, your cursor is just a 1 column wide selection!

As I've been writing this blog post, I've been moving around using the word motions — they're pretty natural for moving around in literal text. \
Another thing that's very natural to want to do in literal text is to delete / change words. \
This combines nicely: I move to some word, and it also *happens* to become my target. Often times, I only need to press a single key to do the action that I decide to do.

The reason why you'd usually want to use `ciw` rather than `ce` in vim, is due to precision. With the former, you can act on a word no matter what, and so by spending an extra keypress, you can allow yourself to spend less mental effort. Interesting tradeoff! \
It only really makes sense because to *see* what you're going to capture with a motion, you need to actively think. What if your editor thought for you?

{{ hr(id="visuality") }}

This is effectively the greatest strength of helix: rather than having to spend mental cpu cycles to figure out what you'd match with a certain motion, you *already* can clearly see what you'd match, *always*. \
Rather than thinking "hmmmm how do I change this word?", you think "I want to act on *this* area... oh wait! it's already selected! I guess I just `c` then!". \
This seemingly small difference ends up merging together to form a *much* nicer editor experience, especially considering that the entire editor is designed around it.

# "normal" semantics

Something that took me a bit to *get*, is the semantic of normal mode in helix. \
I can put it to words most precisely like this: \
In normal mode, as you move, you *replace* your selection by every next motion. \
In *select* mode, you *extend* your selection by every next motion.

Pretty straightforward, isn't it! Might take some time to get used to, but ultimately it's really nice.

Let me present another example of how this model aids you, without you needing to put in extra effort for extra benefit.

The `f`/`F`/`t`/`T` motions replace your selection too! When you `f` towards something, you make a selection from your current cursor position to the character that you aimed for. \
You may use `f` just to move and ignore the selection it makes, \
or you can use `f` to *select* towards something to then delete it, for example. \
Regardless of which action you decide to do, you can make the motion in the exact same way.

# built in "plugins"

Helix has the equivalents of [telescope](https://github.com/nvim-telescope/telescope.nvim), [nvim-surround](https://github.com/kylechui/nvim-surround), [whichkey](https://github.com/folke/which-key.nvim), [lspconfig](https://github.com/neovim/nvim-lspconfig), [easymotion](https://github.com/easymotion/vim-easymotion), [CamelCaseMotion](https://github.com/bkad/CamelCaseMotion), [conform.nvim](https://github.com/stevearc/conform.nvim) built in.

That's pretty nice! But ultimately things being built in is just nice, not really an argument towards an editor. After all, all of the above are really known plugins in the nvim community, so it's very likely a nvim user uses some, or all of them. \
Right?

That point of view misses something.

Neovim plugins are inherently limited by the nvim API. They can only be as good as nvim allows them to be. \
Some of them may employ some hacks, or less than pretty implementation details that end up making a plugin feel *like a plugin*. \
Something that was tacked onto a foundation that didn't think ahead to work nicely with that plugin's behavior. \
As someone who made 7 plugins, I encountered this fact very closely.

## telescope

An amazing plugin, fwiw. Very extendable, configurable, powerful. \
*However*, it does so much while built *on top* of neovim, that it's noticeable in usage. \
When I launch telescope, I can distinctly see two rendering stages: first, the window appears. Second, it's filled with text.

The two stages happen slowly enough where I can clearly notice them, and it's always been jarring to me. I've since tried to switch off telescope, by discovering [fzf-lua](https://github.com/ibhagwan/fzf-lua), but turns out it's even slower in terms of the UI updates (because it uses terminal mode).

On the opposite hand, *helix*'s "telescope" is built in, and when you press the hotkey to open it, all of the UI happens *at once*. It *opens*, rather than *loads in*, is how I can describe it. And god does that feel incredible!

## whichkey

Neovim's whichkey uses key timeout to reveal itself, which I've already explained being quite uncomfortable to use. But without key timeout, you get no whichkey. \
This is not a point against `folke`, it's a point against a plugin that is better served by being built in. \
And omg, it sure is, in helix!

*Immediately* as you press a key that has submappings, you get shown a whichkey menu, on the right side of the screen. \
Because there's no key timeout, you can look at it for as long as you want to! This is how I was able to *discover* a large part of helix. \
Generally I use `anki` to memorize all keybinds, but with a nice interface to teach me, there was far less need to do so.

Because the mapping system is designed having this menu in mind, you create all of your mappings in a hierarchical fashion. \
So, rather than mapping `mf` to something, you map `f` in section `m` to something. \
Nvim's whichkey had to create this system for itself.

### nicer registers

Another neat thing it does, that completely solves an issue nvim otherwise has, is that when you access a register (via `"` by default), it *shows you* the contents of filled registers in that same whichkey menu!

In nvim, you use `:reg` to look at a register's contents. But you only ever need to look at a register's contents *right* before using it in some way, and so helix does exactly that!

I was never a fan of the whichkey idea overall, but helix's implementation was very helpful and improved my opinion a bit :3 \
Ultimately still, it's a bit too much UI for me to see jump around, so I removed it once I memorized every mapping. \
In [my fork](https://github.com/Axlefublr/helix), I added an option to disable the whichkey menu, but keep it for registers, so that's what I've been using.

## surround

Because nvim text objects are actually just visual selections in disguise, using text objects is often jumpy and hard to look at. \
When you combine that with nvim-surround, which uses even *more* janky movement + change approaches, the experience ends up looking very "pluginy".

The helix built-in surrounding mechanism doesn't have to work within such restrictive rules, as it's built into the system to begin with. \
There's something really elegant and powerful it lets you do, thanks to that!

Say you're writing markdown, and want to make a word **bold**, retroactively. \
In nvim, you would do `ysiw*ysiw*`. \
You *almost* could `ysiw*.`, but I believe it moves your cursor onto one of the `*`, making you first move before being able to dot repeat. \
So basically, that's kinda cumbersome. And that's just a situation where you *can* use the same motion twice; that's not always the case.

For example, it isn't the case if you wanted to surround a `word` in such a way, that you would get: `{ ['word'] }`. \
In nvim, this is kind of a pain, because you keep needing to use a different text object every time: `ysiw'ysa']ysa] ysa }`. \
Even if we assume you use a plugin that makes that last `ysa] ysa }` into a `ysa]{` ([targets.vim](https://github.com/wellle/targets.vim), I think?), we still get left with: `ysiw'ysa']ysa]{`.

First, it's an obscene amount of characters to press. \
But more importantly, you use THREE different text objects here, and one of them uses `i` rather than `a`. \
Doing this has always been quite a pain for me, as you need to think about what you're doing with considerable amount of focus.

{{ hr(id="consistency") }}

How does this work in helix? Remember that "motions replace your selection" thing? What if I told you surrounding did this too? \
Genious! Of course it would! \
Due to this convenient consistency, executing the two examples I just gave is very simple.

The first one, considering that you already selected the word, is: `ms*ms*`. \
After the first surround, your selection updates to *contain* the new characters you just added. So you don't need to "fix" your selection or anything! \
You unfortunately can't dot repeat that second `ms*` in helix yet, which is quite annoying :c \
In nvim it wasn't that big of a deal because you had to fix your position anyway, but it would be way nicer to have in helix because you **don't** need to fix the selection.

The second example is just fantastic, especially in comparison: `ms'ms[ms ms{`. \
All you need to do is *just* keep saying what you want to surround with. \
Not having to fix your position, or use a different text object, REALLY helps here! \
I can do this blindly without really needing to focus much, so it's quite a big win.

## autopairs

Not much to say about this one, it just works! \
You can define what your autopairs are globally, and then also define them per-language, in a very obvious configuration format. \
Very possible that I just picked a wrong plugin, out of the million that exist, but in neovim adding an autopair was a chore, that also ended up working differently from all the other autopairs already enabled by default, in a jarring way.

## lsp

Oh my FUCKING god do I despise neovim's lsp integration. \
Somehow despite all of my experience and knowledge, when it comes to the editor, I still see lsp configuration as half magic. \
Fucking 5 or so different plugins doing god knows what in magicky ways, requiring you to do a lot of guesswork to configure in any non-standard way (that lsps sometimes require (rust-analyzer)). \
With this structure, hanging on by a thread, it still takes up like 50 or so milliseconds off your startup time, which is pretty horrendous. You then may spend a lot of effort on optimizing that startup time, too. Which is a craft of its own...

I forgot to mention this, but helix starts instantly. If you get all the neovim plugins that "exist" in helix, and lazy load them properly, neovim will still take longer to put you into a buffer, than helix will. You get everything, while not sacrificing speed.

And the lsp integration is *really* simple and lovely. In your `languages.toml`, you define which lsps you want to use (you can just specify multiple of them per-language if you want!) and what arguments they should be given to start properly. \
That's in theory, at least. In practice it's probably already done for you by helix contributors. So all you need to do is install the lsp so it's in your `$PATH`, and blammo — the lsp just works. I'll repeat that: in the general case, to get an lsp to work, you *just* install it. Often times you won't even really need to configure it.

Formatting is similarly simple: you define how to call it (if it's not already done for you), and then can use `:fmt` or the auto-format option (configured globally *and* per language, also).

There's no non-lsp diagnostic sources like in none-ls or nvim-lint, unfortunately. But it's a tradeoff I'm willing to make, because I only lost fish shell diagnostics, which aren't that important to me anyway. I'm sure it'll get added in eventually, though!

# registers part 2

Let's go back to registers for a bit. The way that helix handles them is what initially made me *consider* switching to begin with.

In nvim, when you yank into a named (`[a-z]`) register, you *also* yank into the default register (`"`). I always found this annoying! \
Imagine a situation where you yanked something, and are travelling through the file to paste it elsewhere. \
As you travel, you notice another piece of text that you want to *also* take with you.

What do you do in this situation? Correct, you eat sand and annoyingly come back for it later. Or use my plugin that I didn't publish.

You might present uppercase registers (`[A-Z]`) as a solution, but appending, rather than overwriting a register doesn't necessarily solve the problem. \
If the important yank you made is a characterwise yank, appending to it most likely isn't what you want at all. \
Even if both the important yank 1 and important yank 2 are linewise, there's no guarantee that they are related; you might want to paste them separately. \
Separating them after the fact seems like an annoying workaround.

And let's not forget: you yanked to the *default* register. So you can't even append to it anyway. \
And you can't retroactively move it into another register, without coming back and reyanking, or without using [my plugin](https://github.com/Axlefublr/edister.nvim) that I did publish.

All of this hassle because yanking, even to a named register, still touches your default register. A non-unfuckable situation that I ran into often (enough to have made to plugins to work around it). \
In helix, it *obviously* just yanks to the register you specified. Duh! You specified a register, that register should be used. Pretty straightforward!

Now that same situation is colored in a different light: \
You yank something important, then find *another* thing you want to yank. \
You just select a register and continue on! :D \
And as you collect these important yanks, when you access yet another register, helix will tell you which registers you're already using, implying what the free ones are. \
So (if you pay attention) you won't even accidentally overwrite an important yank!

{{ hr(id="price-of-mistakes") }}

Using registers is always an *active* decision for me. Because of this, it's common that I yank something, and then think "wait nevermind, I actually want this in register `x`". This "nevermind" thing happens most commonly when my current default register contents *aren't* important.

In neovim, if you "misyank" something, you then need to re-specify the area you targeted. \
`yiw` then "whoops" then `"xyiw`. Get more and more annoying, the more complex text objects you use.

Even if you used visual mode for your selection, you can't *just* reyank: the selection is gone, and you have to press `gv` to restore it.

In helix, after you yank something, you can re-yank without any such hassle. `y` "whoops" `"xy`. That's it.

The selection actually staying like that is a common "omg that's so nice" exclaim I do to myself, about helix. \
But there are places where, at least at first, it's just really weird!

{{ hr(id="strange-consistency") }}

Remember how your cursor is just a single-width selection? Helix stays consistent to the idea of selections mattering more than your cursor in this interesting way: \
By default, `a` appends after your selection, and `i` prepends before your selection.

Yes, really! *Not* your cursor!

This means that if you have `word` selected, and your cursor is at `wor█`, \
when you press `i`, you will start inserting here: `|word`, \
rather than here: `wor|d`. \
Same case with `a`, just in the opposite direction.

"Ok that's just ridiculous" you might say, and at first I agreed with you! \
While I did, I remapped `a` and `i` to get the behavior we both expect:
```toml
a = ['collapse_selection', 'append_mode']
i = ['collapse_selection', 'insert_mode']
```

But after using helix for some time, I actually removed these bindings! \
Helix is *very* consistent with acting on selections specifically, and when you want to act on a *cursor*, it expects you to press `;` first, to collapse selections. \
Having `a` and `i` fall out of this rule became annoying, rather than helpful, as I got used to pressing `;` when I need to.

But aside from consistency, turns out it's just kinda nice!

{{ hr(id="ai") }}

Say you're writing a sentence, and as you move through it, you got some word selected. \
You can pick whether you want to insert before *or* after the word, without having to adjust your position! \
It's just `a` or `i`, depending on where you want to end up :D

Even more powerfully, this behavior of `a` and `i` makes *surrounding* things really easy. First you press `i` and type the left delimiter in, then `a` to type in the right one. \
I remember there was a very hyperspecific feature in nvim-surround to let you do exactly this; here it's just part of the editing model, which is fascinating!

However, I half-think that this `a`/`i` behavior is more of a side effect than intended behavior — your selection is updated when typing in with `a`, but isn't with `i`. So far that hasn't given me problems, but it *does* feel like a bug. Maybe will get fixed someday!

I'll admit, all this selection business takes a while to get used to, but actually now I prefer it. Even in my fish shell "helix mode", I re-made this behavior, that's how much I like it!

{{ hr(id="pasting") }}

Another example is `p` and `P`. Yep, they also paste after and before the *selection*. \
With them though, the behavior isn't particularly mind-boggling, and just ends up feeling correct immediately.

Say you have `word` selected, with your cursor on `█ord`. \
After you yank, to multiply `word`, you just press `pp` as many times as you need. \
If you tried doing that in vim (same cursor position, after you yank with `yiw`), you'd end up with `wwordwordord`. \
I massively prefer *just* pressing `p` mindlessly, rather than having to first move to the end of the word, or press `Pppp` instead. \
And if I wanted to multiply backwards in vim, `word` vores itself in an even more horrifying way: `worworwordddword`.

# aha! the perfect editor!

After all this glazing you might rightfully think "helix must be perfect!". \
Is that the case? FUCK NO!

The big downside of helix is its lack of maturity and slowness of development. \
First of all, it doesn't have a plugin system. \
Yep, not at all! \
Even something like kakoune, which design is *against* the idea of plugins, *does* have a plugin system. \
Your *shell* has a plugin system. That itty bitty program that you click clack commands in. \
Your fridge probably has a plugin system. \
Helix doesn't!

And because of this, you're bound to encounter problems that are currently unsolvable, as generally a plugin for that behavior would already exist, or you'd write your own to get it. \
This state of affairs makes it infuriating to use helix sometimes, because the `:trim` command has been bikeshed for over a year, and some seemingly basic lsp methods are not implemented, leading to some lsps literally not working!

The example I'm immediately thinking of, is the Scala lsp not working because it can't show its startup message. This may be niche, but papercuts do pile up!

Have I already mentioned that you can't complete words from the document in helix ~~stable~~ master? Yeah... \
This feature is completely working and finished, *but* is floating around as a PR for seemingly no reason at all. \
As I like to say: “if we consider all the unmerged completely finished and working PRs, we have a functioning editor!”. \
On one hand, we already have way more than enough features to have a really fucking good editor. \
Because it's designed so well, you tend to be able to work around the missing parts not too difficultly. \
But still, the more I learn about helix and its decisions, the more I think it holds itself back just to reach unecessary perfection; never trading "perfect" for "done". \
And basic editor features are so important, that I'd rather see them get merged asap, and then evolve over time into becoming perfect. But maybe that's unrealistic?

## is this a worthy tradeoff?

But consider all the elegance of the editor I've discussed so far — was the slowness of development a necessary evil to achieve it? Is it the reason I even switched to helix in the first place? It's a difficult spot to be in emotionally, as I really do care deeply about this editor now 🥺.

Regardless, the issues that come from not having a plugin system is why many people, including me, decide to make a personal [helix fork](https://github.com/Axlefublr/helix).

Editing the source code of a text editor to get the missing features sounds, truthfully, insane. And like a lot of effort. That's what I thought, at least!

Turns out, the developmental slowness has made the helix codebase *really* good. \
I don't think I'm *that* good at rust, however I keep getting surprised at how *easy* it is to add new features in.

Especially coming from a *lot* of neovim configuration, I expect to go on a whole adventure to try to make something work, and then get surprised when my endeavor cost me 5 minutes at most.

# a happy dev adventure

One feature I have in my fork (all features are listed in the readme) is that you can remove the statusline.

Vaguely remembering how the statusline works in nvim, if you were to remove it, it would be unreasonable to think to enable it back on at runtime, without reloading neovim as a whole. \
I kept this assumption with helix — I put in only slight effort, *just* to be able to disable the statusline using a config option, not caring enough to implement the ability to switch it back on dynamically. \
After finishing the feature, I wanted to see *how much* things break if I try to `:toggle should-statusline`. \
Imagine my surprise when nothing broke 🤯 \
Matter of fact, the statusline showed up with all the information up-to-date, no less!

This flabbergastment of mine never stops: with every new feature I try to add, it generally goes far better than I expect. Traumatized by neovim configuration I guess! \
But it *makes sense*. Of *course* an editor that feels clean and consistent is also written in a clean and consistent way.

This is all to say that if you decide against using my fork for whatever reason, consider forking it yourself anyway. You'll be pleased to see the features you want be easily addable, and considering how slow helix development moves, you won't need to rebase on upstream that often. Because of this ease though, you'll gain a habit of screaming "WHY ISN'T THIS IN CORE ALREADY", like I did :>
