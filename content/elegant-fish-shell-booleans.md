+++
title = 'elegant fish shell booleans'
date = '2024-10-24'
+++

I recently *finally* figured out how booleans work in fish shell, and turns out they're very elegant!

You can use `set` to set variables to values; you can set them to `true` or `false` too. \
If your editor supports fish shell as a language, it will probably highlight them as booleans.

What are they? Some special types, or special syntax introduced by the shell?

Not at all actually, they're *just* strings, like `asdf` and `the` would be.

{{ hr(id="test") }}

You usually use `test` to check for various common equality comparisons, in shell scripting.

```fish
if test $var = 'thingy'
   echo yeppies
end
```

If the string in variable `var` is equal to `thingy`, output `yeppies`.

{{ hr(id="if-semantics") }}

Let's examine more closely *why* we need `test` here to begin with.

What `if` does, is take in arguments, call those arguments as a command, and execute the inner block of code if the command's exitcode is 0.

`test $var = 'thingy'` are the arguments to the `if` command. \
If `test` executes successfully (meaning `$status` is 0 after it exits), `if` will execute the code block, which is `echo yeppies`

What this means is that both `test` and `if` are not that special. They're just commands doing command stuff. \
So you can use *any* command in an `if`, to check for its exitcode:

```fish
if rg 'struct' args.rs
   echo yeppies
end
```

If there is the word `struct` in the file `args.rs`, output `yeppies`. \
I'm saying all this to highlight: `if` *just* wants to execute a command as its condition, that's it.

{{ hr(id="command") }}

You can even store the command in a variable:

```fish
set -l command rg 'struct' args.rs
if $command
   echo yeppies
end
```

The variable `$command`, will expand to be `rg 'struct' args.rs`, so this is basically equivalent to the codeblock above, where we *don't* use a variable.

{{ hr(id="booleans") }}

Coming back to booleans:

```fish
set -l var1 true
set -l var2 false
if test $var1
   echo yeppies
end
if test $var2
   echo yeppies
end
```

Despite `true` and `false` looking colorful, and therefore seeming "special", in your editor, \
they are actually both *just* strings, so both if statements here will execute their codeblock.

{{ hr(id="false") }}

I always thought, for some reason, that `false` will evaluate to, well, *false* in `test`, but now that I think about it, why would it?

The real gold lies in something interesting: both `true` and `false` are also *commands*. Fish shell built in commands. \
`true` always returns with exitcode 0, `false` always returns with exitcode 1.

So by setting variables to (completely normal strings) `true` or `false`, you also happen to set those variables to commands that can be executed in `if` conditionals.

```fish
set -l condition true
if $condition
   echo yeppies
end
set condition false
if $condition
   echo yeppies
end
```

Will essentially expand to:

```fish
if true
   echo yeppies
end
if false
   echo yeppies
end
```

It's much more clear this way, why `true` and `false` seem special! \
It's entirely *just* because they also happen to be commands that you can call to get your wanted exitcode, \
**not** because fish shell uses them as special syntax.

