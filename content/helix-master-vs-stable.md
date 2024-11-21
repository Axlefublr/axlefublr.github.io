---
title: helix master vs stable
date: 2024-10-11
draft: true
---

When considering switching to helix master, I went through all the commits after stable. \
Here are the new features I discovered.

## pickers

There's a picker refactor: most pickers now have columns, that you can search directly. \
Here's how the bindings picker looks like now:

![image](https://gist.github.com/user-attachments/assets/4b7258e1-6512-467b-bedf-19e08a7d2020)

And thanks to columns, it's much nicer to use! \
You can type in `%column_name` to start searching in that column. \
So you can search by *specifically* documentation by using `%doc`:

![image](https://gist.github.com/user-attachments/assets/1215b1e2-d1dc-48ca-b548-f3c10f4dc1e0)

Or just `%d`! The column matcher just needs to be unique, from the start of the column name; you don't have to use the full name.

![image](https://gist.github.com/user-attachments/assets/7634fe14-4305-490f-9985-9b8d027d2b25)

You can combine a normal search with a column search:

![image](https://gist.github.com/user-attachments/assets/a55988ea-f93c-43a3-822e-f9749b266ea8)

And even multiple:

![image](https://gist.github.com/user-attachments/assets/5d0dc842-93af-4322-be17-0e7c4d98df41)

The `global_search` action used to prompt you for the search in the commandline area, and then show the results at once. Now it is an interactive picker, and shows results as you type in the regex pattern:

![image](https://gist.github.com/user-attachments/assets/8e756791-8fc0-4c2c-979f-bcf71db66615)

Also has a column you can additionally filter by:

![image](https://gist.github.com/user-attachments/assets/397f03ec-a045-4aaa-ada2-2d8dfb307c84)

There's now an [`inline-diagnostics`](https://docs.helix-editor.com/master/editor.html#editorinline-diagnostics-section) option in the `[editor]` section, that lets you have [lsp-lines](https://github.com/ErichDonGubler/lsp_lines.nvim)-like diagnostics (without being super buggy, like that nvim plugin is).

![image](https://gist.github.com/user-attachments/assets/7163bea5-9914-4ea3-966c-755811dc58f4)

Lsp snippets: when lsps complete extra text (default parameters for example), it now actually appears in the completed suggestion. It used to strangely dissapear. \
This snippet implementation is not full; you still can't go to the previous / next node in the snippet, for example.

Extra information for suggestions now appears at the top / bottom of the screen, rather than being squished to the side of the suggestion box. So generally you get to look at it a bit more normally.

![image](https://gist.github.com/user-attachments/assets/4a025a85-1a75-459e-b11c-0fc8365c554d)

In mappings, you can now use @ to bind to literal keypresses. For example `'mw' = '@miw'`.

Subword movement actions. Search for them with `sub_word` in the keybind picker. I went with using `gw`, `ge`, `gb` (the first two are mapped by default, I moved those default mappings elsewhere).

Hardlinks were broken: if you wrote to a hardlink, it wouldn't update the other file. This was fixed in master.
