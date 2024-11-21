---
title: why I hate lua
date: 2024-07-05
---

What I value most in a language is *functionality* — what and how easily the language lets me do things. \
No wonder! That's what programming is kinda for: *doing cool things more easily*. \
And here's lua, that doesn't even have an abstraction over something as simple as `.split()`. Ridiculous.

## the horrid std

Lua has a very weak standard library. You'll find yourself reimplementing functionality that you'll be flabbergasted isn't there to begin with. \
Sure, some of that functionality might be simple to implement, but it shouldn't be my job. \
Imagine doing the equivalent of implementing the `print` function in every new project you create. It's disheartening.

Not only is the standard library weak, but it's also not ergonomic. \
Whenever you want to push a value into an array (more precisely a list / vector), the way that you're used to doing so in basically every language on earth is this: `my_array.push(value)`.

Well duh! Right? Right...

## and here come tables...

The way you do that in lua is `table.insert(my_table, value)`.

"Oh it sure looks like the expanded version of the method syntax, so you can just do `my_table:insert(value)` and it'll work surely" — yeah that's what I wished too. \
What is sometimes an implementation detail in some languages, is actually *the* way you're supposed to call, what should be by all accounts, a *method*.

Insert is not a method for any table, which is the obvious thing that you want, no. It's a *function* that just happens to be in the `table` module. \
This kind of idea repeats with stuff like `math.abs()`, but math being stored in ~~the balls~~ a module is a pretty common thing across languages, and not too insulting. \
An *action* you're doing on an *object* being a function is **very** insulting.

## the solution is even worse!

You know how you can fix this though? With inscrutable symbol soup!

```lua
local my_table = setmetatable({}, { __index = table })
```

And now you can do:

```lua
my_table:insert(value)
```

Yipee 🥳 right? \
Yeah fuck no! Why is this bullshit not the default? It is not reasonable at all to insert that symbol soup every time you want to make a table, just to get slightly better syntax. \
The confusion you'll get from (most) people is simply not worth it. \
So I'm stuck between a rock and a hard place.

You could I guess make this:

```lua
local function get_reasonable_table()
    return setmetatable({}, { __index = table })
end
```

But even with this, this is a bit stupid. \
Remember, we're not doing this to inherit some table in our designed app, we're doing this *just* to have slightly better syntax. \
So I once again argue that it's not worth it.

## locality is a fatality...

Notice how both that variable and function use the `local` keyword? \
Yeah that's because both of those would otherwise be *global* by default. Because that's a very good idea for a default /s

To reiterate: why in living and dying hell are those not *local* by default? \
And then you'd maybe opt into globality via `global`?? Autohotkey does it and it's great there! A bunch of other languages make globality more annoying to use, which is overall a good idea hehehe :3

Or perhaps they use the concept of `const`, which covers a lot of the cases for wanting globality anyway.

Additionally confusingly, you need the `local` keyword even if you're not in a block / function. \
Because without it, the variable won't just be global in the file you're working on, nope — it's going to be global to *everywhere*. \
So you put the `local` keyword to make the variable "local to the module". This language is a god's mistake.

## stupid ass convention

But we continue on: if a feature exists, makes sense to use it. \
Even `goto`s have their use, I'm sure. \
(While writing I mentioned `goto`s as a joke, only to then realize they actually exist in lua... Yes, `goto`s, but **not** the FUCKING CONTINUE STATEMENT. Yes, really. I'm way ahead of you and I *have* been crying about it, don't worry.)

The convention for variables and functions in lua seems to be using `snake_case`. Alrightie then.

Another convention, suggested by the lua language server, is to make at least the first letter of global variables uppercase.

What I think is suggests is `SCREAMING_SNAKE_CASE`. \
This of course makes a lot of sense for global variables that are meant to be `const`s. \
("pinky promise `const`s", because the concept of actual `const` in lua doesn't exist, and all variables are mutable) \
But not all global variables are meant to be `const`s, many of them will be global just so that they can be accessed from anywhere conveniently. So we can't just lie and use that upper case, because it *will* confuse programmers looking at our code, when we modify (after instantiation) what looks like an immutable variable.

What other choice do we have? `PascalCase`? No!! That's obscene. The convention is *still* "variables use underscores to separate subwords", so why would we suddenly change that because of *globality*? It makes no sense.

And, I can understand a pascal case function, cause that does appear in some languages (like C#). A pascal case variable is really out of the blue, considering it's not the usual case in lua, or any other language I've worked with.

So what am I left with? `This_stupid_bullshit`. To satisfy both conventions, I have to do something as ridiculous as that. I will shoot anyone that argues for this absolute strawman of a casing convention.

If we bring it back to using `SCREAMING_SNAKE_CASE` disregarding the `const`-related confusion, then we'd have to name *functions* in that case too. Which is a whole another level of cursed.

```lua
local thingy = YEP_JUST_A_GLOBAL_FUNCTION(params)
```

What the fuck.

## display: hide; that shit

You might say "well you could just ignore the suggestion by the language server" — I wholeheartedly reject that notion. \
The lua language server will be used by the overwhelming majority of people that program in lua. \
The majority sets a standard, and a good programmer follows standards (yes, even if they're stupid!). So I can't just ignore it.

I haven't looked at much lua code aside from that I wrote (cause remember, I fucking despise the language!), but it seems like the convention is to just never use global variables lol. Which is, like, fair.

## functions

Lua uses the concept of "first level citizen functions". Or however that concept is properly worded. \
Basically, functions are just values that you can assign to variables, and store in tables. \
That kind of behavior is actually pretty cool, and makes sense for the language. The problem I have with them is the syntax.

Think of basically any language that uses the concept of closures. \
Rust's `|x|` (stupid but consise), python's `lambda`, javascript's `(x) => {}`. \
All three differentiate functions and closures, fwiw, so they might not be good examples, but my point is that you can make syntax for closures that is nice and consise. \
If you wanted to interpret functions and closures in the same way, using `fn` / `def` / `fun` / etc works just as well.

Then why in living and dying hell does it *have to* be `function`. Bitch, you might as well write an entire poem just to define an anonymous closure in lua by that point. God am I mad at this in specific. \
Because of how *neat* the concept of "functions are also closures" is in lua, it's very very commonly used. \
And any time you need to pass some closure as an argument, you have to boilerplate it with typing out `function`. \
Just `fun`, `def`, `fn`, `func` would all work! I'm sure you might have a distaste for some of them, but you'll probably see at least one to be "alright as a closure starter". So yeah, it genuinely doesn't need to be that long.

## think of the children!

> "Oh, lua is made to be readable and approachable to newbie programmers!"

That's a good benefit! \
There are for sure some things that make sense to change to make them more approachable to newbies. \
An example of that could be `{}` vs indentation in python. \
I have genuinely heard `{}` confusing people, and making it harder for people to read code. \
This is not the case for me anymore, but I do remember the time when I was just starting out, where that definitely *was* the case. `{}` were harder to visually parse.

You ***cannot*** make this same argument for `function` vs `func`. \
The latter is just as visible as the former, and just as obvious *literally* as soon as you define your first function in lua. \
Your second function forward, you now have to *deal with* typing out the entirety of `function`.

## you have an lsp for a reason

> "Oh just autocomplete it"

Yeah no. Most (lua) programmers are on vscode, and that editor will generally not be fast enough to make it worth it to complete `function`. \
By the time the suggestion window appears and you pick the correct thing (cause also remember, most people don't realize they can fuzzy search their suggestions), you would have already typed in `function`. \
So the result is you always just typing in `function` yourself. `func` would reduce that by **half**, and would be quite ergonomic.

## patterns

Actually, forget that "approachable to newbies" point, cause it's obscene in its own sense. Let's talk about lua *patterns*. Do you know how they work? I sure don't.

Instead of implementing regex, lua has its own flavor of pattern matching with its `gsub`s and the like. In those, you have to use lua patterns. \
They *sometimes* resemble regex, but for some odd reason don't try to fully emulate it. \
And let me also mention that you can't just `my_str:contains(literal_string)` — you'd have to use `gmatch` I believe, and that has to use lua patterns, meaning you can't just conveniently raw compare a substring. \
What a travesty.

The main point behind not implementing regex is that it would double the size of the lua binary. \
I personally couldn't give two shits, but I can see it as an important concern, so I don't have much against that point.

So, what I'd expect is for lua to match the syntax of regex, but just have a bunch of missing features. \
If that was the case, there wouldn't be a "patterns" section in this rant. \
But no, lua decides to use its own syntax, that you now have to learn, even if you're familiar with regex.

Why do I then not complain about rust not having regex? \
That's because it has enough features to make it very viable to not use it — you have other ways of achieving the same text parsing objective. And you have the obvious `.contains()`.

## who made this 💀

Coming back to syntax: why does `then` need to exist? \
It's used in `if` statements for example. Statements that only take a single expression as a condition anyway. \
So it's already as clear as day where that condition ends! Because it's required to end after a single expression! \
And so, everything else should be considered the body of the `if` statement, and there's no confusion. \
And yet we stil have `then` for some odd reason. Fish shell very comfortably doesn't!

I don't like the `end`-type "bracket keywords" in programming languages, but I can see how it's needed in something like fish shell. \
It does not need to be so in a "proper programming language" which I expected lua to be.

## why even use lua then

Worst thing about lua is the fact that every goddamn configurable thingy in existence seems to immediately go for this piece of shit language, resulting in both my window manager and text editor to be configured in it. \
Lua is unfortunately my most used programming language, however mad I am at this fact being the truth.

Other languages that I've seen people dislike are lisp and haskell. \
I haven't tried either of them, maybe I would like them maybe not, but regardless, I retain my possibility of avoiding them if I so desire. Lua is fucking inescapable.

> "Oh you might like Fennel then!"

(yes every possible argument made towards me by the reader will start with "Oh")

Good suggestion! I honestly might. \
But having to use a language to fix another is just another proof of lua being a shit language. Kinda like the javascript vs typescript situation.

What I use lua for is configuration. If I want *functionality*, I'll pick fish shell any day over lua. \
So creating a level of abstraction over my configuration-based lua is just an extra complexity that I'll have to *deal* with. Making it not worth it conceptually. \
This is the reason why I haven't rewritten my neovim config in rust (which you can supposedly do, as it turns out).

## P.S.

Everything I've learnt about lua was against my will, so if I'm wrong on some points in this rant, don't be surprised. \
I'm not interested in giving a language I hate more time than I already do, to learn how to circumvent its annoyances. \
This rant is also against *lua*, not against *lua likers*. I believe it's (almost) always better to like something than to not, so if you enjoy the language, you're better off than me!
