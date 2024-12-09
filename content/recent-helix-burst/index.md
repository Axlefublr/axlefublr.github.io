+++
title = 'newest helix master features'
date = 2024-12-10
+++

[As you may know](@/helix-master-vs-stable/index.md), I maintain my own [helix fork](https://github.com/Axlefublr/helix). \
Recently I noticed a huuuuuuuge burst of new commits, and here's what new features I found!

# cd improvement

The `:change-current-directory` (`:cd`) command now more closely mimics the useful behavior of `cd` in the shell: \
With no arguments (`:cd`), it now changes directory to `~` (`$HOME`). \
`:cd -` changes directory to the most recent one. So, if you `:cd ~/Documents` and then `:cd ~/Downloads`, `:cd -` will put you back into `~/Documents`.

# auto-trim trailing whitespace with a caveat

The `insert_newline` action now trims that line's trailing whitespace.

So if you make custom mappings for `insert_newline`, or far more likely when you press <kbd>Enter</kbd> in insert mode, that line's trailing whitespace is automatically removed.

{{ video(path="trim-trailing-whitespace") }}

While this is cool to have and will fix most trailing whitespace moving forward, the issue is the *moving forward* part. \
Somehow we still don't have a `:trim` command or on-write hooks that would trim for us, so this on-newline behavior won't help you clean up a codebase — only to not mess up (as much) when writing a new one.

# auto-continue line comments

Our second on-newline feature! Although this one doesn't depend on the `insert_newline` action, and will happen even if you use something like <kbd>o</kbd> / <kbd>O</kbd>

{{ video(path="extending-comments") }}

This feature has actually been in core for about a month I think, but I didn't rush to rebase to get it because honestly I didn't want the feature. \
I used it back in neovim, and I found it to annoy me more often than help me. An extra `// ` / `# ` / etc to type in is not as much effort as possibly having to *fix* them. \
Additionally, in neovim the operation felt very wonky visually.

The wonkiness is not an issue in helix, as this is a *core* feature, not something that was tacked on, so that's crossed out. \
However the "annoying" part may still remain, as after ~1 minute of fiddling with it, I found a bug when you try to extend a comment upwards, when you're at the first line in the file:

{{ video(path="weird-bug") }}

Interestingly, there is no option to disable this auto-comment-extending behavior.

# path completions

Now you get autocomplete for filepaths! :D

{{ video(path="filepath-completions") }}

If you were using my fork prior, you already had these! \
I've been following the PR that implements this feature for a month or two, and I had it merged into my fork. \
I'm quite surprised to see it get merged into core! So now that's one less responsibility on my fork.

But also, before this point, there was a bug where you could only complete a single filepath on a given line. If you tried to complete another path, the completions wouldn't show up. \
This is no longer an issue!

This feature is enabled by default, but if you wanted to disable it, set the `path-completion` option in the `[editor]` section to false.

# default comment token

For unrecognized languages (if you just open a file with no extension and no inferrable text, for example), the comment token used to be `//`. Now it is `#`. \
Makes sense, considering that more often than not, an unrecognized file is a config file that *probably* uses `#` for comments (if it even supports them).

# conf "language"

You might notice that the syntax highlighting for random `.conf` files is now gone. \
This happened because of a commit that (as it says in the commit message) “Fix language configuration for .conf files”. \
“Fix” my ass.

Put this in your languages.toml to bring it back:

```toml
[[language]]
name = "hocon"
file-types = [
  "conf",
  { glob = "**/src/*/resources/**/*.conf" },
  { glob = "*scalafmt*.conf" },
  { glob = "*scalafix*.conf" },
]
```

# safer paste

If you use `paste` in insert mode, that makes an undo "checkpoint":

{{ video(path="paste-checkpoint") }}

I needed to press <kbd>u</kbd> three times there, to undo the second line. \
Previously, a single <kbd>u</kbd> would undo the whole line.

This is pretty great! It's somewhat often that I start typing in a line and then paste from my system clipboard, only to realize I pasted in the wrong thing. Now after I undo, I *don't* lose the extra text I typed in before the paste.

About clipboards...

# configurable clipboard providers

Previously, helix figured them out automatically. \
Now, you have an option to set them explicitly.

## bit from the docs:

`clipboard-provider` — Which API to use for clipboard interaction. \
One of `pasteboard` (MacOS), `wayland`, `x-clip`, `x-sel`, `win-32-yank`, `termux`, `tmux`, `windows`, `termcode`, `none`, or a custom command set.

### `[editor.clipboard-provider]` Section

Helix can be configured wither to use a builtin clipboard configuration or to use a provided command. \
For instance, setting it to use OSC 52 termcodes, the configuration would be:

```toml
[editor]
clipboard-provider = "termcode"
```

Alternatively, Helix can be configured to use arbitary commands for clipboard integration:

```toml
[editor.clipboard-provider.custom]
yank = { command = "cat",  args = ["test.txt"] }
paste = { command = "tee",  args = ["test.txt"] }
primary-yank = { command = "cat",  args = ["test-primary.txt"] } # optional
primary-paste = { command = "tee",  args = ["test-primary.txt"] } # optional
```

For custom commands the contents of the yank/paste is communicated over stdin/stdout.

{{ hr(id="wither") }}

> Yes it really said “wither”

This is of course cool for the (I think unlikely?) situation if your clipboard provider didn't work, but what I find more interesting is the *custom* clipboard provider. \
As you see in the example they specify, you can straight up just use a random file. \
Interestingly, that's a hack to get your latest register to stay across helix sessions (unless helix deletes the file on exit, which would be mean).

If you use my fork though, you don't need to bother doing that, as I merge in a PR that adds persistence to various things, including your default register.

However: the "primary selection" (register `*`) is something that I never use, so I could potentially change it to give me fairly arbitrary behavior. \
I could easily write a script that would act as my `*` register, and do different things on paste / yank.

For the most part, actions like `shell_pipe`, `insert_output`, `append_ouput` along with command harps (my fork's feature) already provide you this "easily accessible script" behavior, but maybe you can come up with a strong enough contender that would make sense to put directly as the `*` register.

Share your ideas with me on [bluesky](https://bsky.app/profile/axlefublr.bsky.social) or [discord](https://discord.gg/bgVSg362dK), I would love to hear them.

# configurable default register

The default register used to be just `"`. Used when you don't explicitly specify any other register.

But *now*, there's an option in the `[editor]` section called `default-yank-register` that you can set to override that.

Aside from path completion, I feel this is the biggest "fuck yes finally!" recent feature.

In neovim, there was some option (don't remember lol) to let you use your system clipboard as your default register. So if you set it, you could just `y` directly into the clipboard and `p` directly from the clipboard.

I used it, actually! And I loved it. \
And now, helix has it too 🥳

However, unsurprisingly, it's *better* — you aren't restricted to just set the `+`/`*` registers as your default ones, you can use whichever you want! \
It could be just `a`, or even the `/` register, terrifyingly: that would give you the behavior that all yanks set your current search.

Or remember that custom clipboard provider feature from the last section? Nobody is stopping you from combining these two features, and setting your default register to behave differently somehow. \
Default register set to `*`, and default provider for `*` is your custom script. ??? -> profit.

Currently, <kbd>y</kbd>¹ yanks to default register for me, and <kbd>Y</kbd> to the system clipboard. It was more of a workaround than preferred solution, since now I'll just set `default-yank-register` to `+` to sync it to my clipboard, and I'll never need to worry about it again 🥳

> ¹ this is technically a lie, because it's <kbd>s</kbd> and <kbd>S</kbd> that are my yanking hotkeys (I hate pressing <kbd>y</kbd>), \
> but the example would be less clear if I said it to begin with

Keep in mind, the default register using your clipboard directly *may introduce overhead*. \
Meaning: some operations, especially when you yank / paste in bulk, may slow down; sometimes considerably. \
But I'm saying this from experience in *neovim*, and even then it was only noticeable in the most obscene of situations. \
So, it's something to consider, but not something to particularly worry about.

# word-bounded autosearch

<kbd>*</kbd> lets you search for your selection. But it slightly changed!

*Now* it places `\b` around the selection to only match it word-bounded. In other words, if you <kbd>\*</kbd> on `word`, you will no longer match `wordword`.

The default behavior of <kbd>\*</kbd> changed to the new action called `search_selection_detect_word_boundaries`. \
The *old* behavior is in the action called `search_selection` and can be accessed by pressing <kbd>alt+*</kbd>.

Arrest me: I was *just* booting up to complain that I don't like this new default behavior because `\b` doesn't work for non-word characters... \
But they thought of this!

If you select `*just*` and press <kbd>\*</kbd>, it will search for `*just*`. \
If you select `just` and press <kbd>\*</kbd>, it will search for `\bjust\b`.

So it's actually being smart about word boundaries, and only applies them when they would *work*. \
Bravo, Vince.

{{ hr(id="bravo") }}

All of these new features are now also available in [my fork](https://github.com/Axlefublr/helix) 🥳

I liked writing this blog post, so I might continue making these "recent new helix features" blog posts in the future.

Want to make sure to not miss them? Add [this link](/rss.xml) to your RSS reader!

Don't have an RSS reader? I recommend [nom](https://github.com/guyfedwards/nom).
