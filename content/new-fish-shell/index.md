+++
title = 'fish shell 4.0.0'
date = '2029-01-09'
draft = true
+++

New fish shell major version is coming out!

To give myself a better reason to read the entire [release notes](https://fishshell.com/docs/4.0b1/relnotes.html), I want to mention / go over the changes that I found the most interesting in this blog post. \
This is not at all a comprehensive list; if you want one, read the link above.

First of all, fish shell got rewritten from c++ to rust.

`fish_should_add_to_history` function lets you decide which commands to add to history or not.

`--command` flag for `abbr` allows you to make command-specific abbreviations. Basically, you get to make subcommand aliases but with the power of your *shell*, not having to rely on the program to support aliasing.

Take a look at this:
```fish
if test -n $variable
  echo success
end
```

Previously, if `variable` is *unset* (rather than an empty string), fish would error out — `test -n` expects an argument after. \
It's the reason why it's good practice to surround the variable with `"` like `"$variable"`, so that it resolves to a single argument even if the variable is unset.

Now `test -n` and `test -z` *don't* require an argument after, so passing an unset variable is now valid. \
Interestingly, only those two options. All the other options work as they did before.

This feature is not enabled by default, you'd enable it with `set -Ua fish_features test-require-arg`.

`bind` recognizes more modifier combinations and differentiates more keys, for example it can tell apart <kbd>Tab</kbd> and <kbd>ctrl+i</kbd>.

Also, a more convenient syntax for specifying the key like <kbd>ctrl-x</kbd> is now available. \
Previously you had to use `bind -k` for that, now you don't have to.

Omg, I literally needed this so bad: `history append` lets you append a command to fish history, without executing it.

`<?/path/to/file` tries reading the file as input; if reading fails for any reason, outputs from `/dev/null` (which should be an empty string). \
Basically, a way to say "read this file if you can, but don't fail if you can't".

New `path basename -E` is like doing `path change-extension '' (path basename /path/to/file.txt)` — `/path/to/file.txt` gets turned into `file`.

Math `--scale-mode` now allows you to choose how the output value's decimal points are handled. `truncate` / `round` / `floor` / `ceiling`.

Abbreviations can now expand after `command`. As in, `command ls` will expand similar to how *just* `ls` would. \
They now also do after `;` and `|`. But if you put a `|` right after a command without a space in between, you're a freak.

`funcsave`ing a function indents the body.

If there is no second job, `fg %2` won't activate the latest one, and will instead fail.

Depending on your terminal, now <kbd>shift+enter</kbd> will insert a newline rather than execute the command, and <kbd>ctrl+backspace</kbd> & <kbd>ctrl+delete</kbd> works.

In vim mode, the cursor will no longer be beyond the end of the line. I kinda got used to my cursor being 2 character after the end of the line at this point, lol.

Also, `%` motion, as well as functions to `f`/`t` to the matching bracket. 
