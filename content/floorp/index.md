+++
title = 'bye bye firefox'
date = '2025-04-05'
draft = true
+++

You might reasonably expect me to have switched off from firefox due to the recent privacy concerns drama. \
~~But it was me, Dio!~~ But I actually could not care less about my privacy. Matter of fact, I think it's strange and silly to expect your browser to be private about your data. The web is so huge that I can't get how this can ever be an expectation or even a hope.

The actual reasons are somewhat muddy, and because of that I couldn't collect enough energy to actually start the switching process: my firefox switch took an ABHORRENT amount of energy, and repeating that again felt quite scary.

1. My partner really loves this browser, and wants me to try it out a lot
2. It allows you to change hotkeys
3. It has built in workspaces
4. Browser level css is "better" (I didn't quite know in which way yet, but trusted it)

# css

The browser level css configuration turned out way better than advertised. Trully stunning! \
Whatever I try to do just works practically immediately, everything is obvious and straightforward. \
It took me *multiple days* to get firefox just right, and yet I'm almost there on floorp after a few measly hours. That's wild! \
To be fair, there's likely a big bias at play: after cssing firefox, I've gotten a lot better at doing so, and am now judging floorp as if I have the same level of knowledge as I did *before* cssing firefox — this is not the case and it's likely my *knowledge*, **not** the browser that makes it that easy, but still I'm very happy! :D

To get started cssing the browser, the process is quite a bit more discoverable. \
Right click on the workspaces button to enable the menu bar. In it, press CSS -> CSS -> Create Browser CSS file.

In the prompt shown to you, you will create a css file that you'll be editing to customize the browser ui. \
You can then find the file to edit it as you wish, by clicking "Open CSS Folder" in the same menu. \
Unlike the usual firefox userchrome configuration though, you *don't* have to restart the browser for the changes to take effect! \
Just press <kbd>alt+r</kbd> to reload your css file into floorp whenever you make a change in it.

Not having to restart the entire browser to check every change is an *incredible* help for productivity. Although for other things that I might wanna restart my browser for, there's another built in hotkey to do so: <kbd>ctrl+alt+r</kbd>.

Here is [my css file](https://github.com/Axlefublr/dotfiles/blob/main/floorp/CSS/floorp.css). I thought I might as well share it since I'm talking about it, but I give no guarantees that it will be helpful, well written, documented, or even work for you.

# workspaces

I used sidebery for a single reason: "worspaces". It calls it "panels", but no matter. It implements them via hiding and unhiding tabs as you move through the panels (not by default; it's an option), so you can probably imagine how hacky it feels to use. \
Hilariously, it's still faster and smoother than workspaces were on vivaldi. \
Well, floorp has workspaces built in! And they are completely and incredibly smooth!

Sidebery requires you to show the sidebar for it to work. Pretty weird ngl, considering that it *does* allow you to set hotkeys for basically every action. So, I did css to `display: none` the sidebar, while it still technically was "on" — that allowed me to use the hotkeys as expected. \
The downside to doing this is that any time I wanted to add a new workspace (panel), I had to dig into my userchrome.css by finding it in firefox browser devtools, remove the line the removed the sidebar to enable it back, add the workspace, and then remove the sidebar again. \
I don't create workspaces that often thankfully, but yeah it was definitely a pain.

In floorp, you simply get a small thingy to the left of your tabs, that shows the icon of the current workspace (as well as its name, optionally)

![](./workspaces.webp)

It's tiny by default, so I made it a bit bigger with css. \
Unfortunately in terms of icons, you have to pick among the predefined ones: you can't blammo an emoji or svg, how I'd love to be able to do. But good enough! \
A hack you can probably use, and one that I might try out, is to *name* the workspaces using emojis, and *remove* the icon with css. That way you'll only see the name of the workspace, which just so happens to be an emoji of your choosing.

‼️oh actually emoji idea works very well

You can configure your hotkeys! Quite a few of them, at least. Now I get to have <kbd>alt+h</kbd> / <kbd>alt+l</kbd> go the previous / next tab, without having to do an xremap hack to achieve that. Your remapping choices aren't even close to the power of something like vivaldi, though. Still, much better than not being able to to begin with, like in base firefox.

For example, while you can set hotkeys to go to the previous / next workspace, that's all you get. Yes, somehow there aren't built in actions to move the current tab to the previous / next workspace, or really *any* other workspace related action you can bind to, which is very strange. \
I guess I'll give some leeway with the excuse that the project is somewhat new and undercooked. But still, I never get how features this obvious are missed.

‼️switching among workspaces makes panel play hide and seek, for now I display none it; thanks to css reloading (get to this part above) this is viable

![](./actions.webp)

Of course, if I really couldn't move tabs across workspaces with my keyboard, I wouldn't've switched to floorp. No way. \
But a certain feature comes to save me: "Custom actions".

This is probably the coolest part of floorp, for an experienced user. You can set hotkeys to execute your arbitrary javascript! :o

Go to `about:config` and search for `floorp.custom.shortcutkeysAndActions.customAction1`. You set that setting to the javascript you want that action to execute. Then in `about:preferences` -> Keyboard Shortcuts -> Custom Actions, you can set hotkeys to execute those custom actions. \
Interestingly you only get five of them. A bit of an arbitrary restriction, but I'd usually expect that arbitrary number to be 10, not 5. I already use 3 of them — a bit quick to run out of.

My partner wrote this custom action for me, that moves the current tab to the next workspace:

```js
(async () => {
    const currentTab = window.gBrowser.selectedTab;
    const workspaces = await gWorkspaces.getAllWorkspacesId();
    const current = await gWorkspaces.getCurrentWorkspaceId();
    const currentIndex = workspaces.indexOf(current);
    let destinationIndex = currentIndex + 1;
    if (destinationIndex >= workspaces.length) {
        destinationIndex = 0;
    }
    const destination = workspaces.at(destinationIndex);
    await gWorkspaces.changeWorkspace(destination, null, false, true);
})();
```

How to make this move the tab to the *previous* tab is an exercise for the reader. I believe in you.

Another issue I found, that is solved by a custom action: pressing <kbd>Escape</kbd> while in address bar (however many times I tried) *doesn't* focus the page. \
After some version of firefox, I became able to do so, and it's a pretty important workflow bit for me, as I use vimium c. \
I bound the following to <kbd>ctrl+alt+'</kbd>: `document.querySelector("browser[primary='true']").focus()`. Nope, I didn't write this myself either, lol.

Overall, the feeling I get from floorp is that it's a bit undercooked, but gives you the power to cook it to your preference. That is exactly how I approach most software, for better or worse, so that's a green flag for me :3

‼️"oh wow emojis as icons works well", share code to remove the icon as well as share your entire config, saying that it shouldn't be trusted
