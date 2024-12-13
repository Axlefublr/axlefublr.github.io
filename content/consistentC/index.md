+++
title = 'helix is ridiculously consistent'
date = '2024-12-13'
+++

In helix, these two actions exist:

```
copy_selection_on_next_line
copy_selection_on_prev_line
```

The default hotkeys for both of them are <kbd>shift+c</kbd> and <kbd>shift+alt+c</kbd>, respectively.

From the name I instantly assumed: "ah, it's the usual "add multicursor on the next line" and "add multicursor on the previous line"", like practically every multicursor implementation has.

I've talked about the elegant consistency of helix in [this blog post](@/why-even-helix.md); \
turns out the idea of "selections matter, not your cursor" applies here as well!

# showcase

{{ video(path="lines") }}

You create a new cursor below the current *selection*, **not** the current line!

So you could possibly make use of this by selecting two lines, and then effectively make cursors on every other line.

The usecase that comes in mind immediately is some long `if` chain, where the bodies are all a single line, \
and you want to change something in all (or some) of the conditions / bodies.

{{ video(path="something-else") }}

To be fair, I have yet to use this for anything, but found the consistency really cool. \
Being able to express editing desires in such an elegant way is part of why I switched to helix!
