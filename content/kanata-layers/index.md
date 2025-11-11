+++
title = 'erotic meta feet'
date = '2025-11-11'
+++

I just figured out a longterm annoyance I've been having with how kanata does layers, and I want to share my findings with you.

Sequences of key presses and releases are really hard to mentally parse when in word form, so I will establish a syntax for expressing those. \
<kbd>e↓f</kbd> — hold down <kbd>e</kbd>, and while still holding it down, press and release (i.e. tap) <kbd>f</kbd>. \
<kbd>e↓f↓j</kbd> — hold down <kbd>e</kbd>, and while still holding it down, also hold down <kbd>f</kbd>, and while still holding *both* <kbd>e</kbd> and <kbd>f</kbd> down, press and release (tap) <kbd>j</kbd>. \
<kbd>e↓[jk]</kbd> — while holding down <kbd>e</kbd>, individually tap <kbd>j</kbd> or <kbd>k</kbd>; maybe repeatedly. \
<kbd>e↓f↓w↓[jk]f↑[jk]</kbd> — one after the other, start holding down <kbd>e</kbd>, <kbd>f</kbd>, <kbd>w</kbd>. While holding them all, tap <kbd>j</kbd> or <kbd>k</kbd> however many times. Then, release <kbd>f</kbd> but not the other two held keys. While continuing to hold <kbd>e</kbd> and <kbd>w</kbd>, tap <kbd>j</kbd> or <kbd>k</kbd> however many times. \
<kbd>efw↓[jk]</kbd> I hold down <kbd>e</kbd>, <kbd>f</kbd> and <kbd>w</kbd> in *unspecified order* and tap <kbd>j</kbd> or <kbd>k</kbd>. So this might be one of: <kbd>e↓f↓w↓[jk]</kbd>, <kbd>e↓w↓f↓[jk]</kbd>, <kbd>f↓e↓w↓[jk]</kbd>, <kbd>f↓w↓e↓[jk]</kbd>, <kbd>w↓e↓f↓[jk]</kbd>, <kbd>w↓f↓e↓[jk]</kbd>. \
This notation implies that the order of the presses shouldn't matter.

# our setup

I'm going to create an example config to explain things with. \
We will have three layers: meta, erotic, feet. \
You enter the “meta” layer with <kbd>w↓</kbd>, the “erotic” layer with <kbd>e↓</kbd>, and the “feet” layer with <kbd>f↓</kbd>. \
I name them funnily so that their absurdity can help you make them apart more easily 😌 \
“Ah yes we have layer a b and c” would get confusing quick imo.

```
(deflayermap (default)
   w (layer-while-held meta)
   e (layer-while-held erotic)
   f (layer-while-held feet)
)

(deflayermap (meta))
(deflayermap (erotic))
(deflayermap (feet))
```

The meta layer is actually for the meta modifier. You might also know it as super, or the windows key. \
Erotic is the custom layer that has hotkeys for the browser: it makes <kbd>j</kbd> and <kbd>k</kbd> (<kbd>e↓[jk]</kbd>) select the above/below tab, to do further actions on. \
Feet is the “navigation” layer, remapping <kbd>h</kbd>, <kbd>j</kbd>, <kbd>k</kbd>, <kbd>l</kbd> to the arrow keys (<kbd>←</kbd>, <kbd>↓</kbd>, <kbd>↑</kbd>, <kbd>→</kbd>).

```
(deflayermap (default)
   w (multi lmet (layer-while-held meta))
   e (layer-while-held erotic)
   f (layer-while-held feet)
)

(deflayermap (meta))
(deflayermap (erotic)
   j (select-next-tab)
   k (select-prev-tab)
)
(deflayermap (feet)
   h left
   j down
   k up
   l right
)
```

The meta layer itself doesn't need to have any mappings yet — we use it as just the meta modifier for now. \
But for that to work, we use `multi` to *also* send the meta key down press, as long as we keep holding <kbd>w</kbd>.{{fn(i=1)}}

Erotic and feet don't refer to a real modifier though: they are custom layers, not inherently made to mean anything, and are given purpose by us. \
In erotic, I use `select-next-tab` and `select-prev-tab` as abstractions for however the fuck I actually do that; the *real* right side action is unimportant here, and would make things confusing for no benefit :p

# wonderful

, now we can <kbd>f↓[hjkl]</kbd> to get arrow keys, <kbd>e↓[jk]</kbd> to select tabs in the browser, and <kbd>w↓[???]</kbd> to use my compositor (window manager / DE / etc) actions. \
But I start running out of convenient to press hotkeys on just <kbd>w↓</kbd> for my compositor; I now want to make <kbd>wf↓</kbd> hold some of the more “complex” compositor actions.

In niri (the compositor), I make hotkeys on <kbd>meta+left</kbd>, <kbd>meta+down</kbd>, <kbd>meta+up</kbd>, <kbd>meta+right</kbd>. \
The idea here is that when I combine the meta and feet layer, I should effectively get meta+arrow keys, so I can make mappings on those on the compositor side, and it'll work!

But now, we are seemingly in *both* the layer meta and the layer feet. \
That's not exactly true!

In kanata, only one layer is your “current” one.
*But*, kanata remembers the previous layer that you *were* in.

<kbd>w↓</kbd> moves you into the meta layer. It is now your current one. \
You start holding <kbd>f</kbd>; the meta layer doesn't have a mapping for <kbd>f</kbd>, so the *previous* layer is looked up for a mapping.  \
Our previous layer is “default”, which indeed has a mapping for <kbd>f</kbd> — it moves us into the feet layer. \
Where now, this is our sequence of layers: default → meta → feet

In our meta+arrow hotkeys, we rely on the meta layer to hold the meta modifier for us, and on the feet layer to translate <kbd>h</kbd>, <kbd>j</kbd>, <kbd>k</kbd>, <kbd>l</kbd> into arrow keys. \
This only works well because the meta layer *doesn't* have any mappings on <kbd>h</kbd>, <kbd>j</kbd>, <kbd>k</kbd>, <kbd>l</kbd>.
Let's look at the behavior if it *did* have them.

```
(deflayermap (meta)
   h (cmd fish -c "notify-send nuh-uh")
   j (cmd fish -c "notify-send nuh-uh")
   k (cmd fish -c "notify-send nuh-uh")
   l (cmd fish -c "notify-send nuh-uh")
)
```

Now when I <kbd>w↓f↓[hjkl]</kbd>, I can do the more special compositor actions as expected. \
But if I do <kbd>f↓w↓[hjkl]</kbd> (notice the different order), all I get is a nuh-uh!

This shows that in kanata, *the order matters*. \
We are *kinda* in meta and feet at the same time, but each layer has its own precedence, and this changes depending on which we started holding down first.

Of course I'm not going to have hotkeys specifically to “nuh-uh” me, so here there's conveniently no issue. \
It starts to *actually* come into play once you want this kind of composite layer with *two*, rather than one fake layer. \
Before, we relied on the meta *modifier* — the actual hotkeys were set in niri, not kanata. \
And so, only one fake / custom layer was actually in use — feet. \
Right now, we'll make hotkeys directly in kanata, expecting <kbd>ef↓</kbd> — two custom layers together, erotic + feet.

<kbd>e↓[jk]</kbd> selects the next / previous tab. \
Let's make <kbd>ef↓[jk]</kbd> *extend* that selection.

{{video(path="select-extend")}}

...Where do we put those hotkeys at, though? \
Erotic doesn't hold some actual modifier for us, and neither do feet. \
To put our new hotkeys, we **have to** make a composite layer!

```
(deflayermap (erotic-feet)
   j (extend-select-next-tab)
   k (extend-select-prev-tab)
)
```

> You can actually name this layer whatever you want! \
> I just choose to name it `erotic-feet` because it makes it clear which other two layers it's combining.

Currently though, we can never *get* to this layer. \
<kbd>e↓f↓</kbd> results in default → erotic → feet \
<kbd>f↓e↓</kbd> results in default → feet → erotic \
Once again, kanata is *sequential*, **not** combinatory, so we need to explicitly put ourselves into erotic feet somehow.

> For clarity: we are relying on the hotkeys set in the default layer, to move us into erotic or feet.
> Erotic doesn't have a mapping to enter feet — but the default layer does, and so it's used as a fallback. \
> The opposite is also true: feet don't have a mapping to enter erotic — default layer does. \
> But the default layer, naturally, doesn't move us into erotic feet.

To do this, you'll supposed to put the “entering” hotkeys into both of the layers that you want to combine.

```
(deflayermap (erotic)
   f (layer-while-held erotic-feet)
   j (select-next-tab)
   k (select-prev-tab)
)
(deflayermap (feet)
   e (layer-while-held erotic-feet)
   h left
   j down
   k up
   l right
)
```

If we <kbd>e↓</kbd> then <kbd>f↓</kbd>, we get into erotic feet. \
If we <kbd>f↓</kbd> then <kbd>e↓</kbd>, we get into erotic feet also.

It is crucial that we put the enterer in *both* layers — if we didn't have the <kbd>e</kbd> hotkey in the feet layer, that would mean that <kbd>e↓f↓</kbd> would work, but <kbd>f↓e↓</kbd> ***wouldn't***. \
What we are trying to express is “while I'm holding both keys”, rather than “if I start holding down these keys in this specific sequence”.
So we need the enterer in both, to make sure it *doesn't* matter what order you start pressing <kbd>e</kbd> and <kbd>f</kbd> in.

# whoops no feet

Remember the compositor meta+arrow keys remaps? \
We were relying on kanata's sequential layer fallback system, to actually get the arrow keys.

<kbd>w↓f↓</kbd> results in default → meta → feet; feet is the current layer, so of course we get our arrow key mappings. \
<kbd>f↓w↓</kbd> results in default → feet → meta; the current layer, meta, *doesn't* have the arrow key mappings, but the previous layer (feet) does. So we fall back on it.

Crucially, this only works because we *have* visited feet. \
But have a look at how we get to erotic feet.

<kbd>e↓f↓</kbd> results in default → erotic → erotic-feet \
<kbd>f↓e↓</kbd> results in default → feet → erotic-feet

With <kbd>e↓f↓</kbd>, we *never* visit feet — we directly jump to erotic feet. \
With <kbd>f↓e↓</kbd>, we *never* visit erotic — we directly jump to erotic feet.

So if we ever happen to rely on the arrow keys hotkeys existing while in the erotic feet layer, it now again matters how we got there: <kbd>f↓e↓</kbd> will work, but <kbd>e↓f↓</kbd> will not. \
That's because <kbd>f↓e↓</kbd> includes feet in its layer sequence, but <kbd>e↓f↓</kbd> doesn't — so it can't rely on the fallback. \
Once again, we want the order to **not** matter.

We might not *need* the arrow keys mappings in erotic feet specifically, but it's good practice to simplify things for the future you, and to always have *all* the children of a composite layer visited — that way, you build mental consistency you can always rely on. \
Yesterday, I realized that you **can** `layer-while-held` multiple layers at the same time, which fixes the issue completely :D \
Let's look at our entire config again, now with the fixes.

```
(deflayermap (default)
   w (multi lmet (layer-while-held meta))
   e (layer-while-held erotic)
   f (layer-while-held feet)
)

(deflayermap (meta))

(deflayermap (erotic)
   f (multi (layer-while-held feet) (layer-while-held erotic-feet))
   j (select-next-tab)
   k (select-prev-tab)
)

(deflayermap (feet)
   e (multi (layer-while-held erotic) (layer-while-held erotic-feet))
   h left
   j down
   k up
   l right
)

(deflayermap (erotic-feet)
   j (extend-select-next-tab)
   k (extend-select-prev-tab)
)
```

Switching to a composite layer now *also* holds for us the requisite layer that we expect to be held. \
<kbd>f↓e↓</kbd>: default → feet → erotic → erotic-feet \
<kbd>e↓f↓</kbd>: default → erotic → feet → erotic-feet

*Now* if we relied on the arrow keys in erotic feet for whatever reason, that would work whether we got to erotic feet with <kbd>e↓f↓</kbd> *or* <kbd>f↓e↓</kbd>, because we visit the other layer “on the way”. \
Notably, the <kbd>j</kbd>/<kbd>k</kbd> to ↓/↑ remaps of feet won't work because our current layer (erotic feet) remaps them. \
Feet is the *fallback*, not the override.

# let's scale this

Awesome! Now that it doesn't matter what order I press my modifiers, I want to expand this idea by making more hotkeys for the browser. \
You saw in the screen recording, that I currently can *select* tabs — after I do so, as a separate action, I can **activate** that tab. \
Would be nice to *also* be able to switch to the previous / next tab directly, without the activation step.
And I want to be able to *move* my selected tabs, also.

So in total, I want: \
<kbd>e↓[jk]</kbd> to select a tab \
<kbd>ef↓[jk]</kbd> to extend the current selection \
<kbd>wef↓[jk]</kbd> to move selected tabs \
<kbd>we↓[jk]</kbd> to *switch* to the previous / next tab{{fn(i=2)}}.

{{video(path="move-switch")}}

Here is our new config, with all the added enterers and combining layers that we need.

```
(deflayermap (default)
   w (multi lmet (layer-while-held meta))
   e (layer-while-held erotic)
   f (layer-while-held feet)
)

(deflayermap (meta)
   e (multi (layer-while-held erotic) (layer-while-held erotic-meta))
   f (multi (layer-while-held feet) (layer-while-held meta-feet))
)

(deflayermap (erotic)
   f (multi (layer-while-held feet) (layer-while-held erotic-feet))
   w (multi lmet (layer-while-held meta) (layer-while-held erotic-meta))
   j (select-next-tab)
   k (select-prev-tab)
)

(deflayermap (feet)
   e (multi (layer-while-held erotic) (layer-while-held erotic-feet))
   w (multi lmet (layer-while-held meta) (layer-while-held meta-feet))
   h left
   j down
   k up
   l right
)

(deflayermap (meta-feet)
   e (multi (layer-while-held erotic) (layer-while-held erotic-meta) (layer-while-held erotic-feet) (layer-while-held erotic-meta-feet))
)

(deflayermap (erotic-meta)
   f (multi (layer-while-held feet) (layer-while-held erotic-feet) (layer-while-held meta-feet) (layer-while-held erotic-meta-feet))
   j (switch-next-tab)
   k (switch-prev-tab)
)

(deflayermap (erotic-feet)
   w (multi lmet (layer-while-held meta) (layer-while-held meta-feet) (layer-while-held erotic-meta) (layer-while-held erotic-meta-feet))
   j (extend-select-next-tab)
   k (extend-select-prev-tab)
)

(deflayermap (erotic-meta-feet)
   j (move-tabs-next)
   k (move-tabs-prev)
)
```

You can see how this scales to be quite unreadable, lol :D

The idea here, though, is that every layer has enough enterers for us to be able to get to the final erotic meta feet layer, *regardless* of the order. \
Also, we visit all the other layers that are logically pressed, on the way — so that we can reduce possible head pain in the future.

This works! But with a caveat... A caveat that actually made me sure this blog post needs to exist.

First, I need to select the initial tab, from which I'll then extend my selection: <kbd>e↓[jk]</kbd>. \
After that, I start holding <kbd>f</kbd> to extend: <kbd>f↓[jk]</kbd>. \
I have selected the tabs that I want to move, so now I also start holding down <kbd>w</kbd>: <kbd>w↓[jk]</kbd>. \
Wonderful. I have moved my tabs. \
Now onto a separate action — I want to *switch* to the next tab: I release <kbd>f</kbd>, and now only hold <kbd>e</kbd> and <kbd>w</kbd>: <kbd>f↑[jk]</kbd>. \
Full syntax: <kbd>e↓[jk]f↓[jk]w↓[jk]f↑[jk]</kbd> (try following along on your keyboard, to grasp the sequence a bit better)

Selecting, extending, and moving works.
But when I release <kbd>f</kbd>, instead of me now switching tabs, I *still* **move** them! Why is that?

It's another instance of how kanata's sequential layer system can end up being annoying. \
`layer-while-held` means “on press, add this layer on top of the stack. on release, remove that layer from the stack”. \
We move through the layers in this sequence: default → erotic → erotic-feet → erotic-meta-feet (I'm omitting the “on the way” layers because they don't matter here, but they are indeed there). \
Erotic makes us a promise: “When you press <kbd>f</kbd>, I will add erotic feet on top of the stack. When you release <kbd>f</kbd>, I will remove erotic feet from the stack.”

That promise (as far as I understand) *is* actually held.
But a *different* promise takes priority. \
Because *erotic feet* tells us: “When you press <kbd>w</kbd>, I will add erotic meta feet on top of the stack. When you release <kbd>w</kbd>, I will remove erotic meta feet from the stack.” \
So when we release <kbd>f</kbd>, erotic feet is removed from the stack as promised. But that doesn't do much because we are not in erotic feet anymore: we are in erotic *meta* feet. \
Since we *are* still holding <kbd>w</kbd>, the promise of having erotic meta feet in the stack is still held up, by erotic feet.

Our *idea* with erotic meta feet, is that **all** of <kbd>w</kbd>, <kbd>f</kbd>, <kbd>e</kbd> need to be held at the same time. \
If one of them *isn't* currently being held, we need to leave the layer(s!) that require that key to be held.

*Kanata*'s idea is to keep the layer in the stack, as long as *the specific key that entered it*, is still being held.

# virtual keys

Turns out, there's a solution! \
What we want, is to make releasing an enterer key leave not only the *specific* layer that it enters, but *any* (and all) layers that logically expect that key to be held.

For this, we can put the `on-release` action at the end of every enterer's `multi`. \
`on-release` is one of the actions that require us to pass a [virtual key](https://github.com/jtroo/kanata/blob/main/docs/config.adoc#virtual-keys) — they are like aliases that can individually respond to a press / release, rather than being a compulsory press+release combo (as far as I understand them). \
So we will make a couple of virtual keys, that explicitly release *all* layers that necessarily include some sublayer. \
For the record, we do not care about the press / release functionality of virtual keys here, we are just required to use them because we need the `on-release` action.

```
(defvirtualkeys
	any-feet   (multi (release-layer feet)   (release-layer erotic-feet) (release-layer meta-feet)   (release-layer erotic-meta-feet))
	any-erotic (multi (release-layer erotic) (release-layer erotic-meta) (release-layer erotic-feet) (release-layer erotic-meta-feet))
	any-meta   (multi (release-layer meta)   (release-layer meta-feet)   (release-layer erotic-meta) (release-layer erotic-meta-feet))
)
```

We can “call” (tap) one of these virtual keys, to explicitly release any (and all) layers that contain a given sublayer. \
Let's add the `on-release` action to the enterers, in the rest of our configuration.

```
(deflayermap (default)
   w (multi lmet (layer-while-held meta) (on-release tap-vkey any-meta))
   e (multi (layer-while-held erotic) (on-release tap-vkey any-erotic))
   f (multi (layer-while-held feet) (on-release tap-vkey any-feet))
)

(deflayermap (meta)
   e (multi (layer-while-held erotic) (layer-while-held erotic-meta) (on-release tap-vkey any-erotic))
   f (multi (layer-while-held feet) (layer-while-held meta-feet) (on-release tap-vkey any-feet))
)

(deflayermap (erotic)
   f (multi (layer-while-held feet) (layer-while-held erotic-feet) (on-release tap-vkey any-feet))
   w (multi lmet (layer-while-held meta) (layer-while-held erotic-meta) (on-release tap-vkey any-meta))
   j (select-next-tab)
   k (select-prev-tab)
)

(deflayermap (feet)
   e (multi (layer-while-held erotic) (layer-while-held erotic-feet) (on-release tap-vkey any-erotic))
   w (multi lmet (layer-while-held meta) (layer-while-held meta-feet) (on-release tap-vkey any-meta))
   h left
   j down
   k up
   l right
)

(deflayermap (meta-feet)
   e (multi (layer-while-held erotic) (layer-while-held erotic-meta) (layer-while-held erotic-feet) (layer-while-held erotic-meta-feet) (on-release tap-vkey any-erotic))
)

(deflayermap (erotic-meta)
   f (multi (layer-while-held feet) (layer-while-held erotic-feet) (layer-while-held meta-feet) (layer-while-held erotic-meta-feet) (on-release tap-vkey any-feet))
   j (switch-next-tab)
   k (switch-prev-tab)
)

(deflayermap (erotic-feet)
   w (multi lmet (layer-while-held meta) (layer-while-held meta-feet) (layer-while-held erotic-meta) (layer-while-held erotic-meta-feet) (on-release tap-vkey any-meta))
   j (extend-select-next-tab)
   k (extend-select-prev-tab)
)

(deflayermap (erotic-meta-feet)
   j (move-tabs-next)
   k (move-tabs-prev)
)
```

Now, the release of an enterer key not only leaves the exact layer that it enters, but *all* layers that we may enter after the fact. \
Finally, with this laborious ass configuration, everything works exactly how we want!

I select the initial tab, then extend selection, then move the selected tabs; I then release <kbd>f</kbd> and FINALLY, I *switch tab* as I expect, rather than continuing to move.

# the power!

The erotic layer, which is actually called “hack” in my configuration, has for a long time felt iffy to me: when I combined it with other layers to do more “complex” actions in my browser, some of the combinations were not understood by kanata, based on rules I wasn't sure existed. \
Now that I actually looked into it deeper, and **solved it**, it feels incredible to finally combine layers naturally, and not have to release all of them to switch to a different combination — I can just use these custom layers like I would be using normal modifiers.

You might not yet have a need to make composite layers like this, but hopefully you now know *how to*. \
If in the future you get a cool idea for a layer that might need composite layers, you can go “this is gonna be a bit annoying but doable” rather than possibly give up on the idea because it “doesn't work”.

# footnotes

{{hn(i=1)}} We don't actually need the meta layer at all currently; omitting it entirely and making <kbd>w</kbd> be *just* `lmet` on the right side would suffice. But we're going to need this layer for later.

{{hn(i=2)}} More specifically, this will skip over unloaded tabs.
