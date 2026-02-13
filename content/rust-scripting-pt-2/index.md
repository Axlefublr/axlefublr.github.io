+++
title = 'rust for... scripting? pt. 2'
date = '2026-02-13'
+++

In my [previous blog post](@/rust-scripting/index.md), I arrived at a rust “scripting” system that I was quite satisfied with :D
But since then, I took upon a journey to learn nushell, and therefore was using it for a lot of my new needs.
For some of them, nushell isn't even the most appropriate tool, but finding reasons to use the language so that I could *learn* it better is a technique I make use of all the time.

One day, I realize I want to edit a rust “script” that I already have.
Wait... how do I do this again?
There's a workspace directory, that is symlinked to one of the directories in the cache directory, glued by a bunch of harps. Right...

When I zoom out, I realize that the system that I have created is a bit overly complicated.
Coming back to using it again, it isn't immediately *obvious* what I'm supposed to do to edit existing work, and amn't quite sure how I'm supposed to start new work.
Initially, I made the system to *ease in* the barrier of entry towards making a new rust program.
I succeeded in doing so somewhat, but there is still a bit of a barrier.

I can design this to be simpler, surely. And thanks to the suggestion of my friend [remrevo](https://github.com/titaniumtraveler), turns out I make it *better* too.

The core *want* that I want to address is that creating a new “script” contains everything it needs in a single file.
More or less.
Then, I want to *store* all of those scripts more or less in the same place, so that they are pleasant and easy to manage.
And, I want to *name* the scripts like I would be naming **scripts**, rather than how I would be naming projects.
Finally, the resulting binary should have the `.rs` extension in its name, so that rust “scripts” are easily differentiated from “normal” binaries, helping me name them neglectfully.

Putting a `consume.rs` binary into `$PATH` is free — almost no chance something else I'll be using in the future will collide with it.
Putting a `consume` binary into `$PATH` is a lot more questionable.
Pretty dang common word!
I'd need to take a real think before choosing such a name, and I don't want to think!

Now, I keep using the word “script” but at this point it's lost most of its meaning.
I've been fine with having a compile step for a while.
I'm okay with having files around it to make it work, but I really don't wanna create a whole ass another directory for a new “script”.
And I'm not actually running *the file*, I'm running the binary that just so happens to be named the same as its source code.

So I say “script”, but I mean “an easily creatable rust binary”.
Doesn't quite roll off the tongue as easily though, so I'm going to keep using the word script.

Rust provides me kinda **exactly** what I want, turns out.
In a rust project, you create a `src/main.rs`, and that is the entry point to your binary program.
You may then also (or instead) create a `src/lib.rs`, and that's the entry point to the *library* side of your program.
You can have both at the same time, or only one of them.

But you can create another special thingymabop: `src/bin/`.
You can create rust files in that directory, and each individual file is its **own binary entry point**, like `src/main.rs` is.

```
src
   bin
      consume.rs
      zat.rs
Cargo.toml
```

This turns out to be much more fantastic than the system I would've designed would be.
In a *normal* project, you just have one binary, and it's automatically named the same as the project is called.
*Some* projects, though, can produce multiple binaries.

Yazi produces `yazi` and `ya`, nushell produces `nu` and a bunch more other binaries for plugins.
In those projects, the “entry points” are defined manually in `Cargo.toml`, but with `srs/bin/` we don't need to bother doing that!
A file with an `.rs` extension existing in `src/bin/` makes cargo automatically register it as a binary entry point.

**So**, to run or compile a “script”, we refer to it with the `--bin` flag.
If I want to `cargo run` the `consume.rs` script, I do `cargo run --bin consume`.
Pretty straightforward!

To make this ergonomic to use, I can put `:sh cargo run --bin %(buffer_stem)` into a [command harp](@/harp/index.md) in my helix{{fn(i=1)}}.
With it, I can express “run the script I am currently looking at” (in my editor), on a singular hotkey that I can put into my muscle memory.

Then to “release” a script, I first do `cargo build --release --bin %(buffer_stem)`, and then `cp` over the resulting binary into the directory where I hold my wks binaries, appending the `.rs` extension at the end.
Pretty simple also.
Especially when compared to my previous wks system!

There, to edit an existing script I would need to import it from a file into the “workspace” directory and use a fancy exporting script so that the compilation is targeted from the cache.
If I wanted to create a *new* script, I would need to first check if I haven't forgotten to export the changes in the workspace directory and then use the once again fancy script for bringing in a blank slate, and symlinking everything into place. \
Wanted to make an ad-hoc binary for ~~a laugh~~ testing? I'd need to point it to a path where the source + Cargo.toml would be exported into, even when I don't care to actually *store* the testing source code long-term. \
It was a mess!

In the new system, I *just* create a new `.rs` file in `src/bin/` and I'm done.

{{hr(id="cargotoml")}}

Next thing that you might see in the file structure, is that all the binaries *share* a singular `Cargo.toml`.
This is fantastic!
Once I `cargo add` some dependency, *all* of my binaries get access to it.
The same version is used for all of them, and crucially the same *compile artifacts* are used across all of them.
So once I compile `clap` once, I won't need to wait for it to compile for my next binary that also uses clap.
This is also neat for my storage space, as rust compile artifacts like to balloon quite a bit!

The binaries also share a single *library* entry point.
```
src
   lib.rs
   bin
      consume.rs
      zat.rs
Cargo.toml
```

This in particular unlocks some incredible power!
I can write abstractions and helpers in `lib.rs`, that I then continue on to use across a bunch of different binaries.
Where at first, creating some sort of binary might be a pain, after a bunch of iteration I'll have made the experience so nice for myself that I might then reach for rust out of *desire*, not only out of performance needs.

In the old wks system, the default template for a new rust script contained a long paragraph of `use`s, for structs and traits that I'm very likely to use in a script.
Having to import only what I need gets annoying quickly if I keep on needing the same things, after all.

But now, I can compact them all into a single
```rust
use wks::prelude::*;
```

To do this, I create a new module in `lib.rs` called `prelude`
```rust
pub mod prelude;
```

I create `prelude.rs`
```
src
   lib.rs
   prelude.rs
   bin
      consume.rs
      zat.rs
Cargo.toml
```

And go ham, `pub use`ing all the imports that I feel like I need most of the time:
```rust
#![allow(unused_imports)]
pub use anyhow::Context;
pub use anyhow::Result;
pub use anyhow::bail;
pub use anyhow::ensure;
pub use clap::Parser;
pub use std::borrow::Cow;
pub use std::collections::HashMap;
pub use std::collections::HashSet;
pub use std::env;
pub use std::fmt;
pub use std::fs;
pub use std::fs::File;
pub use std::fs::OpenOptions;
pub use std::io;
pub use std::io::BufRead;
pub use std::io::Read;
pub use std::io::Seek;
pub use std::io::Write;
pub use std::ops::Not;
pub use std::path::Path;
pub use std::path::PathBuf;
pub use std::str::FromStr;
pub use std::sync::LazyLock;
```

And now all of those modules, structs and traits are all in scope immediately, in all of my scripts.

{{hr(id="alias")}}

Kind of by habit I still made myself a nice alias to make reaching existing and creating new scripts more ergonomic, but it's not even that needed honestly.
I have this simple template file:
```rust
#![allow(unused_imports)]
#![allow(unused_variables)]
#![allow(dead_code)]

use wks::prelude::*;

fn main() -> Result<()> {
    println!("{}");
    Ok(())
}
```

That I store in the repo. I then make a fish alias `wks`:
```fish
function wks
    set -l prevdir $PWD
    if test "$argv[1]"
        cd ~/fes/wks
        rsync ~/fes/wks/template.rs ~/fes/wks/src/bin/"$argv[1]".rs
        helix ~/fes/wks/src/bin/"$argv[1]".rs
        return
    end
    cd ~/fes/wks/src/bin
    set -l picked (fzf)
    test "$picked" || begin
        cd $prevdir
        return 1
    end
    cd ~/fes/wks
    helix ~/fes/wks/src/bin/$picked
end
```

With no arguments provided, it opens `fzf` for me to pick an existing script to edit, and then opens it in helix from the root of the `wks` project.
If I *do* provide an argument, that is the name of the script that I want to create (without the `.rs` extension) by copying over the template file.

I intentionally don't safeguard myself from overriding an existing script this way!
I have a `test.rs` script that I gitignore.
It's meant for various ad-hoc checks and tests.
Any time I want to start testing something from a clean slate, I *just* `wks test` and I'm there!
Whatever the test script used to be before then is wiped clean, and I can immediately get to work.

Recently realized how nice this is in practice.
I was working on a new helix feature as usual, and for some reason `the.get(1..)` was returning me `None`, despite `the` being at least two characters long (in fact more).
I do a simple `wks test` and check out the test case in a “pure” environment, unlike helix, and was able to figure out the core of the issue{{fn(i=2)}} really quickly :D
It was such a pleasant workflow!

The [repo that holds wks](https://github.com/Axlefublr/wks) is public!
Check it out, and maybe make use of the idea for your own nice collection of itty bitty rust “scripts”.

# footnotes
{{hn(i=1)}} “My” is very significant there, `buffer_stem` is a custom command expansion in [my helix fork](https://github.com/Axlefublr/helix). Normally you would need to use `%(file_path_absolute)`, converting it into its basename without the extension in one way or another.

{{hn(i=2)}} The first character was `€`, a multibyte character. `get` on a `&str` returns `None` if you try to index inside of a multibyte character.
To work around this, I used `the.char_indices().nth(1)` to get the index of the second character, and then used that value in the afforementioned `get`.
