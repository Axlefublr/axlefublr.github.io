+++
title = 'a neat helix multicursor trick'
date = '2024-09-01'
+++

You may sometimes want to set a bunch of multicursors on a lot of lines, where just holding `C` and letting key repeat do the job is a bit bothersome. \
So you `mip` to select inside paragraph, and use `s` to set cursors how you usually would with `C`, but with far less keystrokes

{{ video(path="regex-column-showcase") }}

Doing this feels illegal fwiw, but it's actually super valid for this usecase, where you want to act on a certain column, rather than anything that you can regex match by text. \
If you know exactly how many columns you need to match, you could also just `^.{30}` or something, but I generally don't know. \
Regex is really powerful in theory, really fun to learn how to use it more creatively.
