+++
title = 'stop picking on files'
date = '2025-02-16'
+++

When using linux, over time you might naturally arrive at hating guis and preferring a more terminal-centric workflow. \
One thing that always stood out to me as unescapable though, is *picking files*.

Say you wanna send a file on a messenging app, or maybe blammo an image over another one in krita. \
You will most likely need to open a file picker to pick the file that you want, or a gui file manager that will allow you to drag & drop.

Always found it very annoying. \
In my terminal, I have tooling like [zoxide](https://github.com/ajeetdsouza/zoxide), [harp](https://github.com/Axlefublr/harp), [yazi](https://github.com/sxyazi/yazi), `fd`, `rg`, and everything else that makes getting to a file a comfortable experience. \
But when I need to interact with The™ Outside™ World™ (the browser, mostly), now suddenly that tooling is not very helpful. \
What I do now is copy the filepath in the terminal in one way or another, and then paste it in the file picker that gets opened; this is better but still kinda ass to have to do.

Recently my [friend](https://github.com/titaniumtraveler) showed me something incredible: the uri-list.

Executing the following in my shell:

```fish
echo file:///home/axlefublr/i/e/memes/blackbeard-writing.gif | wl-copy -t text/uri-list
```

Lets me go on discord and just <kbd>ctrl+v</kbd> the file.

![](./blackbeard-writing.gif)

I'm fairly certain that this is the mechanic that file managers use for drag and drop functionality, so this solves the need for a gui file manager or a file picker in practically every place 🤯

{{ hr(id="alias") }}

First, it makes a lot of sense to make an alias for this. I'm on fish, so the syntax may differ for you:

```fish
function copyl
  echo file://(realpath $argv[1]) | wl-copy -t text/uri-list
end
funcsave copyl >/dev/null
```

Now I can do something like `copyl blackbeard-writing.gif` while my current working directory is `~/i/e/memes`, and that will resolve to the full path that is required, letting me input a file into my clipboard quickly.

I don't like typing in paths though, so for the usecase of picking a file I will generally use `yazi`. \
I made a mapping that does `copyl` there, and now the process of sending memes or whatever else is really nice! [termfilechooser](https://github.com/GermainZ/xdg-desktop-portal-termfilechooser) begone!

{{ hr(id="filotkis") }}

A more overcomplicated idea, is that now you can put *files* on **hotkeys**. \
Use something like [xremap](https://github.com/xremap/xremap) for hotkeys that put specific files into your clipboard, like above; now you can drive a meme to the ground even faster! This is what you get by learning linux 😎 \
For this idea to be slightly more viable though, I heavily recommend using harp. Changing your xremap configuration every time you wanna update your image hotkeys is bound to be very annoying.

{{ hr(id="screen-record") }}

Also, this is useful for screen recordings. \
Screenshotting tools generally put the image into your clipboard, so you just paste it directly. \
A screen*video*ing tool can't put the video into your clipboard (?), but you can just make it `copyl` the file uri, to be practically the same thing as the file being in your clipboard. \
Very convenient! Now I can make a screen recording and immediately paste it, no need to open a file picker :3
