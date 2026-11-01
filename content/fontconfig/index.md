+++
title = 'fontconfig'
date = '2026-11-01'
draft = false
+++

I have an array of my favorite fonts :] \
I painstakingly specify them in every new program that I start using, and sometimes they get out of sync as I discover new fonts and realize I like those more.
I've vaguely heard of fontconfig, but suuuuuurely an old standard way of configuring fonts is probably too convoluted to actually *use*, right? \
As an example, have you ever tried configuring compose sequences?
It's quite a pain!

In this spot I placed the famous ratatoille gif.
But after looking it for a few seconds, the movement got too annoying, so I can only imagine how annoying it will be for you reading.
You're better off imagining it.

It turned out that fontconfig is pretty dang straightforward :o \
Create the file `~/.config/fontconfig/fonts.conf`, and fill it with this template:
```xml
<?xml version="1.0"?>
<!DOCTYPE fontconfig SYSTEM "urn:fontconfig:fonts.dtd">
<fontconfig>
  <alias binding="same">
    <family>sans-serif</family>
    <prefer>
      <family>Inter</family>
      <family>Noto Sans</family>
      <family>Symbols Nerd Font</family>
      <family>Twemoji</family>
    </prefer>
  </alias>

  <alias binding="same">
    <family>serif</family>
    <prefer>
      <family>Inter</family>
      <family>Noto Sans</family>
      <family>Symbols Nerd Font</family>
      <family>Twemoji</family>
    </prefer>
  </alias>

  <alias binding="same">
    <family>monospace</family>
    <prefer>
      <family>Google Sans Code</family>
      <family>Adwaita Mono</family>
      <family>IBM Plex Mono</family>
      <family>Cascadia Mono</family>
      <family>JetBrainsMono Nerd Font</family>
      <family>Symbols Nerd Font Mono</family>
    </prefer>
  </alias>
</fontconfig>
```

The idea here is defining what the placeholder `sans-serif`, `serif`, `monospace` fonts actually *mean*.
If you didn't know, they aren't *actual* fonts: they are necessarily resolved to some set of other fonts, and can be configured :D

The order *matters*: the font at the top is responsible for rendering all the characters that it knows how to render.
`Inter` in particular supports quite a lot of characters; but not all.
Once it doesn't know how to render something, the next font is tried: `Noto Sans`.
If *it* doesn't know{{<fl id="1" />}} either, the next font is tried, etc etc. \
With the template that I give you, you *prepend* to the default fonts already in place, set probably by your distro.
So once the fonts you have set “run out”, that default list is tried afterwards. \
Unless you're my friend leora, a “tofu” should never be possible.

Next, run `fc-cache -f` to force update your font cache to this new config of yours.
You're done!
To confirm, run `fc-match -s sans-serif | head` and you should see *your* fonts at the top 😌

Now apps that just use `sans-serif` instead of supplying their own fonts, will use *your* preferred fonts automatically! (after a restart of that app)

In anki, eww, foot, fuzzel, gtk, swayimg, vellum, waybar, and More™, I now *just* say `sans-serif` / `monospace`, instead of specifying the entire fonts array over and over again. \
For foot and fuzzel especially, not having to has been revolutionary.
There you can't just set a font size globally, you *must* append a `:size=18` to *every* font that you specify to make it the correct size; when your list of fonts is like 6-7, it gets very annoying.
Now it's just:
```ini
font=monospace:size=18,Twemoji:size=16
```

{{ <hr id="exclusion" /> }}

Having an alias that you can blammo pretty much anywhere and mean the Correct Thing is fantastic as is, but I wouldn't bother writing this post without the next part.

Whenever I'm looking at a new font, it's very important that it doesn't fuck *anything* up.
If it simply doesn't *have* renders for some characters, that's not too bad: my fallback fonts will take care of it.
But if I, for example, really hate how it renders `!` or `,` or `%`, I'm kinda fucked, amn't I?
I might ADORE the letters, but if special characters aren't viable, I'm not following through with that font 😔

Except that now it's no longer an issue!
If you don't like how a particular font renders some symbols, you can *configure that font* to no longer provide its glyphs for *those symbols*.
Or even go more nuclear, and *only* take some symbols from a font, and ignore everything else.

Here's my *actual* fontconfig config (font. config. fontconfig fontconfig):
```xml
<?xml version="1.0"?>
<!DOCTYPE fontconfig SYSTEM "urn:fontconfig:fonts.dtd">
<fontconfig>
  <alias binding="same">
    <family>sans-serif</family>
    <prefer>
      <family>Inter</family>
      <family>Noto Sans</family>
      <family>Symbols Nerd Font</family>
      <family>Twemoji</family>
    </prefer>
  </alias>

  <alias binding="same">
    <family>serif</family>
    <prefer>
      <family>Inter</family>
      <family>Noto Sans</family>
      <family>Symbols Nerd Font</family>
      <family>Twemoji</family>
    </prefer>
  </alias>

  <alias binding="same">
    <family>monospace</family>
    <prefer>
      <family>Google Sans Code</family>
      <family>Adwaita Mono</family>
      <family>IBM Plex Mono</family>
      <family>Cascadia Mono</family>
      <family>JetBrainsMono Nerd Font</family>
      <family>Symbols Nerd Font Mono</family>
    </prefer>
  </alias>

  <match target="scan">
    <test name="family"><string>Google Sans Code</string></test>
    <edit name="charset" mode="assign">
      <minus>
        <name>charset</name>
        <charset>
          <int>0xab</int><int>0xbb</int> <!-- «» -->
          <int>0xb9</int> <!-- ¹ -->
          <int>0xb2</int> <!-- ² -->
          <int>0xb3</int> <!-- ³ -->
          <int>0x2074</int> <!-- ⁴ -->
        </charset>
      </minus>
    </edit>
  </match>

  <match target="scan">
    <test name="family"><string>Adwaita Mono</string></test>
    <edit name="charset" mode="assign">
      <minus>
        <name>charset</name>
        <charset>
          <int>0x21aa</int> <!-- ↪ -->
          <int>0x21a9</int> <!-- ↩ -->
          <int>0x2408</int> <!-- ␈ -->
          <int>0x2421</int> <!-- ␡ -->
          <int>0x221e</int> <!-- ∞ -->
          <int>0x2248</int> <!-- ≈ -->
          <int>0x2260</int> <!-- ≠ -->
          <int>0xbb</int> <!-- » -->
          <int>0xab</int> <!-- « -->
        </charset>
      </minus>
    </edit>
  </match>

  <match target="scan">
    <test name="family"><string>IBM Plex Mono</string></test>
    <edit name="charset" mode="assign">
      <charset>
        <int>0x21aa</int> <!-- ↪ -->
        <int>0x21a9</int> <!-- ↩ -->
      </charset>
    </edit>
  </match>

  <match target="scan">
    <test name="family"><string>Cascadia Mono</string></test>
    <edit name="charset" mode="assign">
      <charset>
        <int>0x2260</int> <!-- ≠ -->
        <int>0xbb</int> <!-- » -->
        <int>0xab</int> <!-- « -->
      </charset>
    </edit>
  </match>
</fontconfig>
```

On the same level as the `<alias>` tags, you can add `<match>` tags.
The order doesn't matter: they affect different *things*.
In them, use a `<charset>` to *only* take specific symbols from the font.
Wrap that `<charset>` in a `<minus>` to mean only **don't** take these symbols, but take everything else.

You can also specify *ranges* of characters like this:
```xml
<range><int>0x2190</int><int>0x21ff</int></range>
```

You can mix and match ranges and standalone characters, no issue:
```xml
<charset>
  <int>0xd9e</int>
  <range><int>0x2190</int><int>0x21ff</int></range>
</charset>
```

My main monospace font is Google Sans Code.
But it doesn't support russian, so I use Adwaita Mono{{<fl id="2" />}} to cover that as a fallback.
Turned out that it also renders a lot of niche symbols, so with these two fonts I'm mostly set :D \
They fuck up *some* of the symbols, that another font does better, though.
For instance, weirdly, Google Sans Code renders ¹²³⁴, but not the rest of the “uppercase” and “lowercase” (lol) numbers.
So I make it stop that 😼 \
I prefer the «» of Cascadia Mono, and ↪↩ of IBM Plex Mono, but kinda nothing else from them.
So I take just what I like :D

This opens up new excitement about fonts as a whole, because the bar is now much lower: it's not “is this font *entirely* good”, but instead “is there *anything* in this font that I prefer over what I already have?”.
The last two fonts I mentioned I only added *today*, actually; specifically and only due to realizing the capability to take *some* things from them, instead of all things.

# debugging

Fontconfig provides you some utilities to make interacting with fonts pretty pleasant actually!

## fc-list

`fc-list` lists all fonts on your system.
When you install a new font, you know the *package* name but not necessarily what the *font* name ends up being. \
For instance, the `ttf-jetbrains-mono-nerd` package.
How is the actual font named?
`Jetbrains Mono Nerd`? `JetBrains Mono Nerd`? `JetBrainsMono Nerd`? Or maybe it's `NF` instead of `Nerd`? Or maybe `mono` is not included altogether? \
To figure this out you can pipe the output of `fc-list` into your editor, and search for your new font there.

```
/usr/share/fonts/TTF/JetBrainsMonoNerdFont-Regular.ttf: JetBrainsMono Nerd Font,JetBrainsMono NF:style=Regular
```
You come across a line like this.
On the left is the actual font *file* name — this is NOT what you care about.
You care about the comma-separated list of aliases on the right of that `:`.
So here we can see that a valid name for the font is `JetBrainsMono Nerd Font` or `JetBrainsMono NF` 😌

If you already precisely know the file name or alias name, you can provide that directly to `fc-list` and skip the “pipe into editor to search” step. \
`fc-list 'GoogleSansCode'` (matches the filename) or `fc-list 'Google Sans Code'` (matches the alias name) results in:
```
~/.local/share/fonts/mine/GoogleSansCode-Italic-VariableFont_wght.ttf: Google Sans Code
~/.local/share/fonts/mine/GoogleSansCode-Italic-VariableFont_wght.ttf: Google Sans Code:style=Bold Italic
~/.local/share/fonts/mine/GoogleSansCode-Italic-VariableFont_wght.ttf: Google Sans Code:style=ExtraBold Italic
~/.local/share/fonts/mine/GoogleSansCode-Italic-VariableFont_wght.ttf: Google Sans Code:style=Italic
~/.local/share/fonts/mine/GoogleSansCode-Italic-VariableFont_wght.ttf: Google Sans Code:style=Light Italic
~/.local/share/fonts/mine/GoogleSansCode-Italic-VariableFont_wght.ttf: Google Sans Code:style=Medium Italic
~/.local/share/fonts/mine/GoogleSansCode-Italic-VariableFont_wght.ttf: Google Sans Code:style=SemiBold Italic
~/.local/share/fonts/mine/GoogleSansCode-VariableFont_wght.ttf: Google Sans Code
~/.local/share/fonts/mine/GoogleSansCode-VariableFont_wght.ttf: Google Sans Code:style=Bold
~/.local/share/fonts/mine/GoogleSansCode-VariableFont_wght.ttf: Google Sans Code:style=ExtraBold
~/.local/share/fonts/mine/GoogleSansCode-VariableFont_wght.ttf: Google Sans Code:style=Light
~/.local/share/fonts/mine/GoogleSansCode-VariableFont_wght.ttf: Google Sans Code:style=Medium
~/.local/share/fonts/mine/GoogleSansCode-VariableFont_wght.ttf: Google Sans Code:style=Regular
~/.local/share/fonts/mine/GoogleSansCode-VariableFont_wght.ttf: Google Sans Code:style=SemiBold
```

This can be useful to easily check which *styles* a particular font has.

We can also ask a different question using `fc-list :charset=1fbf0`: “which fonts are there that can render this particular symbol?” \
You specify the symbol via its codepoint in hex.
In this example, the symbol is `🯰`
```
/usr/share/fonts/Adwaita/AdwaitaMono-Bold.ttf: Adwaita Mono:style=Bold
/usr/share/fonts/Adwaita/AdwaitaMono-BoldItalic.ttf: Adwaita Mono:style=Bold Italic
/usr/share/fonts/Adwaita/AdwaitaMono-Italic.ttf: Adwaita Mono:style=Italic
/usr/share/fonts/Adwaita/AdwaitaMono-Regular.ttf: Adwaita Mono:style=Regular
/usr/share/fonts/TTF/CascadiaCode.ttf: Cascadia Code
/usr/share/fonts/TTF/CascadiaCode.ttf: Cascadia Code:style=Bold
/usr/share/fonts/TTF/CascadiaCode.ttf: Cascadia Code:style=ExtraLight
/usr/share/fonts/TTF/CascadiaCode.ttf: Cascadia Code:style=Light
/usr/share/fonts/TTF/CascadiaCode.ttf: Cascadia Code:style=Regular
/usr/share/fonts/TTF/CascadiaCode.ttf: Cascadia Code:style=SemiBold
/usr/share/fonts/TTF/CascadiaCode.ttf: Cascadia Code:style=SemiLight
/usr/share/fonts/TTF/CascadiaCodeItalic.ttf: Cascadia Code
/usr/share/fonts/TTF/CascadiaCodeItalic.ttf: Cascadia Code:style=Bold Italic
/usr/share/fonts/TTF/CascadiaCodeItalic.ttf: Cascadia Code:style=ExtraLight Italic
/usr/share/fonts/TTF/CascadiaCodeItalic.ttf: Cascadia Code:style=Italic
/usr/share/fonts/TTF/CascadiaCodeItalic.ttf: Cascadia Code:style=Light Italic
/usr/share/fonts/TTF/CascadiaCodeItalic.ttf: Cascadia Code:style=SemiBold Italic
/usr/share/fonts/TTF/CascadiaCodeItalic.ttf: Cascadia Code:style=SemiLight Italic
/usr/share/fonts/TTF/CascadiaCodeNF.ttf: Cascadia Code NF
/usr/share/fonts/TTF/CascadiaCodeNF.ttf: Cascadia Code NF:style=Bold
/usr/share/fonts/TTF/CascadiaCodeNF.ttf: Cascadia Code NF:style=ExtraLight
/usr/share/fonts/TTF/CascadiaCodeNF.ttf: Cascadia Code NF:style=Light
/usr/share/fonts/TTF/CascadiaCodeNF.ttf: Cascadia Code NF:style=Regular
/usr/share/fonts/TTF/CascadiaCodeNF.ttf: Cascadia Code NF:style=SemiBold
/usr/share/fonts/TTF/CascadiaCodeNF.ttf: Cascadia Code NF:style=SemiLight
/usr/share/fonts/TTF/CascadiaCodeNFItalic.ttf: Cascadia Code NF
/usr/share/fonts/TTF/CascadiaCodeNFItalic.ttf: Cascadia Code NF:style=Bold Italic
/usr/share/fonts/TTF/CascadiaCodeNFItalic.ttf: Cascadia Code NF:style=ExtraLight Italic
/usr/share/fonts/TTF/CascadiaCodeNFItalic.ttf: Cascadia Code NF:style=Italic
/usr/share/fonts/TTF/CascadiaCodeNFItalic.ttf: Cascadia Code NF:style=Light Italic
/usr/share/fonts/TTF/CascadiaCodeNFItalic.ttf: Cascadia Code NF:style=SemiBold Italic
/usr/share/fonts/TTF/CascadiaCodeNFItalic.ttf: Cascadia Code NF:style=SemiLight Italic
/usr/share/fonts/TTF/CascadiaCodePL.ttf: Cascadia Code PL
/usr/share/fonts/TTF/CascadiaCodePL.ttf: Cascadia Code PL:style=Bold
/usr/share/fonts/TTF/CascadiaCodePL.ttf: Cascadia Code PL:style=ExtraLight
/usr/share/fonts/TTF/CascadiaCodePL.ttf: Cascadia Code PL:style=Light
/usr/share/fonts/TTF/CascadiaCodePL.ttf: Cascadia Code PL:style=Regular
/usr/share/fonts/TTF/CascadiaCodePL.ttf: Cascadia Code PL:style=SemiBold
/usr/share/fonts/TTF/CascadiaCodePL.ttf: Cascadia Code PL:style=SemiLight
/usr/share/fonts/TTF/CascadiaCodePLItalic.ttf: Cascadia Code PL
/usr/share/fonts/TTF/CascadiaCodePLItalic.ttf: Cascadia Code PL:style=Bold Italic
/usr/share/fonts/TTF/CascadiaCodePLItalic.ttf: Cascadia Code PL:style=ExtraLight Italic
/usr/share/fonts/TTF/CascadiaCodePLItalic.ttf: Cascadia Code PL:style=Italic
/usr/share/fonts/TTF/CascadiaCodePLItalic.ttf: Cascadia Code PL:style=Light Italic
/usr/share/fonts/TTF/CascadiaCodePLItalic.ttf: Cascadia Code PL:style=SemiBold Italic
/usr/share/fonts/TTF/CascadiaCodePLItalic.ttf: Cascadia Code PL:style=SemiLight Italic
/usr/share/fonts/TTF/CascadiaMonoNF.ttf: Cascadia Mono NF
/usr/share/fonts/TTF/CascadiaMonoNF.ttf: Cascadia Mono NF:style=Bold
/usr/share/fonts/TTF/CascadiaMonoNF.ttf: Cascadia Mono NF:style=ExtraLight
/usr/share/fonts/TTF/CascadiaMonoNF.ttf: Cascadia Mono NF:style=Light
/usr/share/fonts/TTF/CascadiaMonoNF.ttf: Cascadia Mono NF:style=Regular
/usr/share/fonts/TTF/CascadiaMonoNF.ttf: Cascadia Mono NF:style=SemiBold
/usr/share/fonts/TTF/CascadiaMonoNF.ttf: Cascadia Mono NF:style=SemiLight
/usr/share/fonts/TTF/CascadiaMonoNFItalic.ttf: Cascadia Mono NF
/usr/share/fonts/TTF/CascadiaMonoNFItalic.ttf: Cascadia Mono NF:style=Bold Italic
/usr/share/fonts/TTF/CascadiaMonoNFItalic.ttf: Cascadia Mono NF:style=ExtraLight Italic
/usr/share/fonts/TTF/CascadiaMonoNFItalic.ttf: Cascadia Mono NF:style=Italic
/usr/share/fonts/TTF/CascadiaMonoNFItalic.ttf: Cascadia Mono NF:style=Light Italic
/usr/share/fonts/TTF/CascadiaMonoNFItalic.ttf: Cascadia Mono NF:style=SemiBold Italic
/usr/share/fonts/TTF/CascadiaMonoNFItalic.ttf: Cascadia Mono NF:style=SemiLight Italic
/usr/share/fonts/TTF/CascadiaMonoPL.ttf: Cascadia Mono PL
/usr/share/fonts/TTF/CascadiaMonoPL.ttf: Cascadia Mono PL:style=Bold
/usr/share/fonts/TTF/CascadiaMonoPL.ttf: Cascadia Mono PL:style=ExtraLight
/usr/share/fonts/TTF/CascadiaMonoPL.ttf: Cascadia Mono PL:style=Light
/usr/share/fonts/TTF/CascadiaMonoPL.ttf: Cascadia Mono PL:style=Regular
/usr/share/fonts/TTF/CascadiaMonoPL.ttf: Cascadia Mono PL:style=SemiBold
/usr/share/fonts/TTF/CascadiaMonoPL.ttf: Cascadia Mono PL:style=SemiLight
/usr/share/fonts/TTF/CascadiaMonoPLItalic.ttf: Cascadia Mono PL
/usr/share/fonts/TTF/CascadiaMonoPLItalic.ttf: Cascadia Mono PL:style=Bold Italic
/usr/share/fonts/TTF/CascadiaMonoPLItalic.ttf: Cascadia Mono PL:style=ExtraLight Italic
/usr/share/fonts/TTF/CascadiaMonoPLItalic.ttf: Cascadia Mono PL:style=Italic
/usr/share/fonts/TTF/CascadiaMonoPLItalic.ttf: Cascadia Mono PL:style=Light Italic
/usr/share/fonts/TTF/CascadiaMonoPLItalic.ttf: Cascadia Mono PL:style=SemiBold Italic
/usr/share/fonts/TTF/CascadiaMonoPLItalic.ttf: Cascadia Mono PL:style=SemiLight Italic
/usr/share/fonts/noto/NotoSansSymbols2-Regular.ttf: Noto Sans Symbols 2:style=Regular
```

I sorted them for your viewing pleasure, but keep in mind that by default the list is annoyingly unsorted and hard to look at.
You might wanna use a simple wrapper that calls `fc-list` and sorts that output:
```fish
function font_list
    fc-list $argv | sort
end
```

## fc-match

Sometimes fonts are hard to tell apart at a glance, and the question of “did my config come through?” becomes annoying to answer.
Run `fc-match monospace` to see your #1 monospace font.
But since you probably configure a *stack*, rather than a singular font, add the `-s` flag to see the entire stack:
```
GoogleSansCode-VariableFont_wght.ttf: "Google Sans Code" "Regular"
AdwaitaMono-Regular.ttf: "Adwaita Mono" "Regular"
IBMPlexMono-Regular.ttf: "IBM Plex Mono" "Regular"
CascadiaMono.ttf: "Cascadia Mono" "Regular"
JetBrainsMonoNerdFont-Regular.ttf: "JetBrainsMono Nerd Font" "Regular"
NotoSansMono-Regular.ttf: "Noto Sans Mono" "Regular"
DejaVuSansMono.ttf: "DejaVu Sans Mono" "Book"
NimbusMonoPS-Regular.otf: "Nimbus Mono PS" "Regular"
NotoSansCJK-Regular.ttc: "Noto Sans Mono CJK KR" "Regular"
CascadiaCodeNF.ttf: "Cascadia Code NF" "Regular"
LiberationMono-Regular.ttf: "Liberation Mono" "Regular"
CommitMonoSuper-400-Regular.otf: "CommitMonoSuper" "Regular"
SometypeMono-Regular.ttf: "Sometype Mono" "Regular"
twemoji.ttf: "Twemoji" "Regular"
Font Awesome 7 Brands-Regular-400.otf: "Font Awesome 7 Brands" "Regular"
Font Awesome 7 Free-Regular-400.otf: "Font Awesome 7 Free" "Regular"
Inter.ttc: "Inter" "Regular"
NotoNastaliqUrdu-Regular.ttf: "Noto Nastaliq Urdu" "Regular"
IBMPlexSans-VariableFont_wdth,wght.ttf: "IBM Plex Sans" "Regular"
NotoSansBengali-Regular.ttf: "Noto Sans Bengali" "Regular"
NotoSansAdlamUnjoined-Regular.ttf: "Noto Sans Adlam Unjoined" "Regular"
NotoSansDuployan-Regular.ttf: "Noto Sans Duployan" "Regular"
NotoSansMongolian-Regular.ttf: "Noto Sans Mongolian" "Regular"
NotoSansWarangCiti-Regular.ttf: "Noto Sans Warang Citi" "Regular"
NotoSansHebrew-Regular.ttf: "Noto Sans Hebrew" "Regular"
NotoSansSyriacEastern-Regular.ttf: "Noto Sans Syriac Eastern" "Regular"
NotoSansThaana-Regular.ttf: "Noto Sans Thaana" "Regular"
NotoSansMath-Regular.ttf: "Noto Sans Math" "Regular"
NotoSansSinhala-Regular.ttf: "Noto Sans Sinhala" "Regular"
DejaVuSans.ttf: "DejaVu Sans" "Book"
DejaVuSerif.ttf: "DejaVu Serif" "Book"
NotoSerifTangut-Regular.ttf: "Noto Serif Tangut" "Regular"
NotoKufiArabic-Regular.ttf: "Noto Kufi Arabic" "Regular"
NotoSansMyanmar-Regular.ttf: "Noto Sans Myanmar" "Regular"
NotoSansGujarati-Regular.ttf: "Noto Sans Gujarati" "Regular"
NotoSansMalayalam-Regular.ttf: "Noto Sans Malayalam" "Regular"
NotoSerifTibetan-Regular.ttf: "Noto Serif Tibetan" "Regular"
NotoSansEthiopic-Regular.ttf: "Noto Sans Ethiopic" "Regular"
NotoSansHanifiRohingya-Regular.ttf: "Noto Sans Hanifi Rohingya" "Regular"
NotoSansLisu-Regular.ttf: "Noto Sans Lisu" "Regular"
NotoSansSoraSompeng-Regular.ttf: "Noto Sans Sora Sompeng" "Regular"
NotoNaskhArabic-Regular.ttf: "Noto Naskh Arabic" "Regular"
NotoSansSymbols2-Regular.ttf: "Noto Sans Symbols 2" "Regular"
NotoSansDevanagari-Regular.ttf: "Noto Sans Devanagari" "Regular"
NotoSansNewa-Regular.ttf: "Noto Sans Newa" "Regular"
NotoSansOriya-Regular.ttf: "Noto Sans Oriya" "Regular"
NotoSerifAhom-Regular.ttf: "Noto Serif Ahom" "Regular"
NotoSerifBalinese-Regular.ttf: "Noto Serif Balinese" "Regular"
NotoSansBalinese-Regular.ttf: "Noto Sans Balinese" "Regular"
NotoSansKaithi-Regular.ttf: "Noto Sans Kaithi" "Regular"
NotoSansKannada-Regular.ttf: "Noto Sans Kannada" "Regular"
NotoSansLepcha-Regular.ttf: "Noto Sans Lepcha" "Regular"
NotoSansSharada-Regular.ttf: "Noto Sans Sharada" "Regular"
NotoSansTifinagh-Regular.ttf: "Noto Sans Tifinagh" "Regular"
NotoSansZanabazarSquare-Regular.ttf: "Noto Sans Zanabazar Square" "Regular"
NotoSansBatak-Regular.ttf: "Noto Sans Batak" "Regular"
NotoSansCaucasianAlbanian-Regular.ttf: "Noto Sans Caucasian Albanian" "Regular"
NotoSansCham-Regular.ttf: "Noto Sans Cham" "Regular"
NotoSansGeorgian-Regular.ttf: "Noto Sans Georgian" "Regular"
NotoSansGrantha-Regular.ttf: "Noto Sans Grantha" "Regular"
NotoSansJavanese-Regular.ttf: "Noto Sans Javanese" "Regular"
NotoSansKhojki-Regular.ttf: "Noto Sans Khojki" "Regular"
NotoSansLimbu-Regular.ttf: "Noto Sans Limbu" "Regular"
NotoSansManichaean-Regular.ttf: "Noto Sans Manichaean" "Regular"
NotoSansOldHungarian-Regular.ttf: "Noto Sans Old Hungarian" "Regular"
NotoSansSiddham-Regular.ttf: "Noto Sans Siddham" "Regular"
NotoSansSignWriting-Regular.ttf: "Noto Sans SignWriting" "Regular"
NotoSansSundanese-Regular.ttf: "Noto Sans Sundanese" "Regular"
NotoSansTakri-Regular.ttf: "Noto Sans Takri" "Regular"
NotoSansTelugu-Regular.ttf: "Noto Sans Telugu" "Regular"
NotoSerifKhojki-Regular.ttf: "Noto Serif Khojki" "Regular"
NotoSansBrahmi-Regular.ttf: "Noto Sans Brahmi" "Regular"
NotoSansCanadianAboriginal-Regular.ttf: "Noto Sans Canadian Aboriginal" "Regular"
NotoSansCoptic-Regular.ttf: "Noto Sans Coptic" "Regular"
NotoSansElbasan-Regular.ttf: "Noto Sans Elbasan" "Regular"
NotoSansGlagolitic-Regular.ttf: "Noto Sans Glagolitic" "Regular"
NotoSansGurmukhi-Regular.ttf: "Noto Sans Gurmukhi" "Regular"
NotoSansHanunoo-Regular.ttf: "Noto Sans Hanunoo" "Regular"
NotoSansInscriptionalPahlavi-Regular.ttf: "Noto Sans Inscriptional Pahlavi" "Regular"
NotoSansInscriptionalParthian-Regular.ttf: "Noto Sans Inscriptional Parthian" "Regular"
NotoSansKharoshthi-Regular.ttf: "Noto Sans Kharoshthi" "Regular"
NotoSansKhmer-Regular.ttf: "Noto Sans Khmer" "Regular"
NotoSansKhudawadi-Regular.ttf: "Noto Sans Khudawadi" "Regular"
NotoSansMarchen-Regular.ttf: "Noto Sans Marchen" "Regular"
NotoSansMiao-Regular.ttf: "Noto Sans Miao" "Regular"
NotoSansModi-Regular.ttf: "Noto Sans Modi" "Regular"
NotoSansNKo-Regular.ttf: "Noto Sans NKo" "Regular"
NotoSansNewTaiLue-Regular.ttf: "Noto Sans New Tai Lue" "Regular"
NotoSansOldItalic-Regular.ttf: "Noto Sans Old Italic" "Regular"
NotoSansOldTurkic-Regular.ttf: "Noto Sans Old Turkic" "Regular"
NotoSansPhagsPa-Regular.ttf: "Noto Sans PhagsPa" "Regular"
NotoSansSylotiNagri-Regular.ttf: "Noto Sans Syloti Nagri" "Regular"
NotoSansTaiViet-Regular.ttf: "Noto Sans Tai Viet" "Regular"
NotoSansTamil-Regular.ttf: "Noto Sans Tamil" "Regular"
NotoMusic-Regular.ttf: "Noto Music" "Regular"
NotoSansAvestan-Regular.ttf: "Noto Sans Avestan" "Regular"
NotoSansChakma-Regular.ttf: "Noto Sans Chakma" "Regular"
NotoSansLao-Regular.ttf: "Noto Sans Lao" "Regular"
NotoSansMahajani-Regular.ttf: "Noto Sans Mahajani" "Regular"
NotoSansMandaic-Regular.ttf: "Noto Sans Mandaic" "Regular"
NotoSansMendeKikakui-Regular.ttf: "Noto Sans Mende Kikakui" "Regular"
NotoSansOlChiki-Regular.ttf: "Noto Sans Ol Chiki" "Regular"
NotoSansOldSogdian-Regular.ttf: "Noto Sans Old Sogdian" "Regular"
NotoSansPsalterPahlavi-Regular.ttf: "Noto Sans Psalter Pahlavi" "Regular"
NotoSansRejang-Regular.ttf: "Noto Sans Rejang" "Regular"
NotoSansSymbols-Regular.ttf: "Noto Sans Symbols" "Regular"
NotoSansTirhuta-Regular.ttf: "Noto Sans Tirhuta" "Regular"
NotoSerifToto-Regular.ttf: "Noto Serif Toto" "Regular"
NotoTraditionalNushu-Regular.ttf: "Noto Traditional Nushu" "Regular"
NotoSerifCJK-Regular.ttc: "Noto Serif CJK JP" "Regular"
NotoSansBamum-Regular.ttf: "Noto Sans Bamum" "Regular"
NotoSansBassaVah-Regular.ttf: "Noto Sans Bassa Vah" "Regular"
NotoSansBhaiksuki-Regular.ttf: "Noto Sans Bhaiksuki" "Regular"
NotoSansBuginese-Regular.ttf: "Noto Sans Buginese" "Regular"
NotoSansCarian-Regular.ttf: "Noto Sans Carian" "Regular"
NotoSansCypriot-Regular.ttf: "Noto Sans Cypriot" "Regular"
NotoSansEgyptianHieroglyphs-Regular.ttf: "Noto Sans Egyptian Hieroglyphs" "Regular"
NotoSansImperialAramaic-Regular.ttf: "Noto Sans Imperial Aramaic" "Regular"
NotoSansIndicSiyaqNumbers-Regular.ttf: "Noto Sans Indic Siyaq Numbers" "Regular"
NotoSansKayahLi-Regular.ttf: "Noto Sans Kayah Li" "Regular"
NotoSansLinearA-Regular.ttf: "Noto Sans Linear A" "Regular"
NotoSansLinearB-Regular.ttf: "Noto Sans Linear B" "Regular"
NotoSansLycian-Regular.ttf: "Noto Sans Lycian" "Regular"
NotoSansLydian-Regular.ttf: "Noto Sans Lydian" "Regular"
NotoSansMeeteiMayek-Regular.ttf: "Noto Sans Meetei Mayek" "Regular"
NotoSansMeroitic-Regular.ttf: "Noto Sans Meroitic" "Regular"
NotoSansMultani-Regular.ttf: "Noto Sans Multani" "Regular"
NotoSansOsage-Regular.ttf: "Noto Sans Osage" "Regular"
NotoSansPauCinHau-Regular.ttf: "Noto Sans Pau Cin Hau" "Regular"
NotoSansRunic-Regular.ttf: "Noto Sans Runic" "Regular"
NotoSansSaurashtra-Regular.ttf: "Noto Sans Saurashtra" "Regular"
NotoSansSogdian-Regular.ttf: "Noto Sans Sogdian" "Regular"
NotoSansTagalog-Regular.ttf: "Noto Sans Tagalog" "Regular"
NotoSansTaiLe-Regular.ttf: "Noto Sans Tai Le" "Regular"
NotoSansTaiTham-Regular.ttf: "Noto Sans Tai Tham" "Regular"
NotoSansThai-Regular.ttf: "Noto Sans Thai" "Regular"
NotoSansYi-Regular.ttf: "Noto Sans Yi" "Regular"
NotoSansAnatolianHieroglyphs-Regular.ttf: "Noto Sans Anatolian Hieroglyphs" "Regular"
NotoSansBuhid-Regular.ttf: "Noto Sans Buhid" "Regular"
NotoSansCherokee-Regular.ttf: "Noto Sans Cherokee" "Regular"
NotoSansCuneiform-Regular.ttf: "Noto Sans Cuneiform" "Regular"
NotoSansDeseret-Regular.ttf: "Noto Sans Deseret" "Regular"
NotoSansGothic-Regular.ttf: "Noto Sans Gothic" "Regular"
NotoSansHatran-Regular.ttf: "Noto Sans Hatran" "Regular"
NotoSansMro-Regular.ttf: "Noto Sans Mro" "Regular"
NotoSansNabataean-Regular.ttf: "Noto Sans Nabataean" "Regular"
NotoSansOldNorthArabian-Regular.ttf: "Noto Sans Old North Arabian" "Regular"
NotoSansOldPermic-Regular.ttf: "Noto Sans Old Permic" "Regular"
NotoSansOldPersian-Regular.ttf: "Noto Sans Old Persian" "Regular"
NotoSansOldSouthArabian-Regular.ttf: "Noto Sans Old South Arabian" "Regular"
NotoSansOsmanya-Regular.ttf: "Noto Sans Osmanya" "Regular"
NotoSansPahawhHmong-Regular.ttf: "Noto Sans Pahawh Hmong" "Regular"
NotoSansPalmyrene-Regular.ttf: "Noto Sans Palmyrene" "Regular"
NotoSansPhoenician-Regular.ttf: "Noto Sans Phoenician" "Regular"
NotoSansSamaritan-Regular.ttf: "Noto Sans Samaritan" "Regular"
NotoSansShavian-Regular.ttf: "Noto Sans Shavian" "Regular"
NotoSansSoyombo-Regular.ttf: "Noto Sans Soyombo" "Regular"
NotoSansTagbanwa-Regular.ttf: "Noto Sans Tagbanwa" "Regular"
NotoSansTamilSupplement-Regular.ttf: "Noto Sans Tamil Supplement" "Regular"
NotoSansUgaritic-Regular.ttf: "Noto Sans Ugaritic" "Regular"
NotoSansVai-Regular.ttf: "Noto Sans Vai" "Regular"
NotoSansWancho-Regular.ttf: "Noto Sans Wancho" "Regular"
NotoSerifDivesAkuru-Regular.ttf: "Noto Serif Dives Akuru" "Regular"
NotoSansTangsa-Regular.ttf: "Noto Sans Tangsa" "Regular"
NotoSansCyproMinoan-Regular.ttf: "Noto Sans Cypro Minoan" "Regular"
NotoSerifDogra-Regular.ttf: "Noto Serif Dogra" "Regular"
NotoSerifOttomanSiyaq-Regular.ttf: "Noto Serif Ottoman Siyaq" "Regular"
NotoSansMasaramGondi-Regular.ttf: "Noto Sans Masaram Gondi" "Regular"
NotoSerifOldUyghur-Regular.ttf: "Noto Serif Old Uyghur" "Regular"
NotoSerifVithkuqi-Regular.ttf: "Noto Serif Vithkuqi" "Regular"
NotoSansChorasmian-Regular.ttf: "Noto Sans Chorasmian" "Regular"
NotoSansGunjalaGondi-Regular.ttf: "Noto Sans Gunjala Gondi" "Regular"
NotoSansNandinagari-Regular.ttf: "Noto Sans Nandinagari" "Regular"
NotoZnamennyMusicalNotation-Regular.ttf: "Noto Znamenny Musical Notation" "Regular"
NotoSansElymaic-Regular.ttf: "Noto Sans Elymaic" "Regular"
NotoSansMedefaidrin-Regular.ttf: "Noto Sans Medefaidrin" "Regular"
NotoSansTest-Regular.ttf: "Noto Sans Test" "Regular"
NotoSansNagMundari-Regular.ttf: "Noto Sans Nag Mundari" "Regular"
NotoSerifMakasar-Regular.ttf: "Noto Serif Makasar" "Regular"
NotoSerifNPHmong-Regular.ttf: "Noto Serif NP Hmong" "Regular"
NotoSerifYezidi-Regular.ttf: "Noto Serif Yezidi" "Regular"
NotoFangsongKSSRotated-Regular.ttf: "Noto Fangsong KSS Rotated" "Regular"
NotoSansKawi-Regular.ttf: "Noto Sans Kawi" "Regular"
NotoSansSunuwar-Regular.ttf: "Noto Sans Sunuwar" "Regular"
NotoSerifHentaigana-Regular.ttf: "Noto Serif Hentaigana" "Regular"
NotoSerifTodhri-Regular.ttf: "Noto Serif Todhri" "Regular"
TT2020Base-Regular.ttf: "TT2020Base" "Regular"
Font Awesome 7 Free-Solid-900.otf: "Font Awesome 7 Free" "Solid"
DejaVuSerif-Italic.ttf: "DejaVu Serif" "Italic"
NotoColorEmoji.ttf: "Noto Color Emoji" "Regular"
```

Seeing it for the first time inspires one... to make it smaller.
That is *so* many Noto Sans, it's kind of insane.
Motherfucker, you aren't even a monospace font!
What are you doing there!

But it *is* helpful still.
God forbid I might need some monospace mongolian in my life, after all.
In my template, we *prepended* our list of fonts to the default list; but you could also *replace* the default list with your one, making the overall stack way smaller.
How exactly to do so is an exercise to the reader 😌

{{ <hr id="precision" /> }}

To debug whether your *character specific* configuration applied correctly, you can use
```fish
fc-match 'monospace:charset=2460'
```
to mean “If I ask to render this character with this font, which font will the symbol *actually* come from?” \
If I run this, I get:
```
AdwaitaMono-Regular.ttf: "Adwaita Mono" "Regular"
```
The `-s` flag works here too, although it feels less useful in this context.

The ergonomics of the above are kind of ahh, though:
1. It's kind of a pain to manually convert a symbol into its hex code
2. Typing out the `:charset=` syntax
3. Typing out `monospace`
4. “Wait, what character did I just specify?”
5. Only one character at a time

So I wrote a nifty wrapper that lets me type in the character as-is, prints that character alongside the result, lets me specify multiple at a time, and assumes monospace unless I specify the `-v` flag.
```fish
function which_font
    argparse v/variable -- $argv
    set -l grouper (if set -q _flag_variable
        echo sans-serif
    else
        echo monospace
    end)
    for char in $argv
        set -l char_index (printf %x \'$char)
        echo $char\t(fc-match "$grouper:charset=$char_index")
    end
end
```

When I run `which_font ← ¤ ⑨ ≺ ♪`, I get:
```
←	GoogleSansCode-VariableFont_wght.ttf: "Google Sans Code" "Regular"
¤	GoogleSansCode-VariableFont_wght.ttf: "Google Sans Code" "Regular"
⑨	AdwaitaMono-Regular.ttf: "Adwaita Mono" "Regular"
≺	AdwaitaMono-Regular.ttf: "Adwaita Mono" "Regular"
♪	AdwaitaMono-Regular.ttf: "Adwaita Mono" "Regular"
```

## test data

I have a file with a bunch of niche symbols laid out in neat sections, that is *very* useful to look at when discovering a new font.
The basic things like letters, numbers, keyboard-ascii special characters I don't include because I'm happy with them as they are, so I focus on improving the nicher symbols.
Might be useful to you too :3

```
← ↓ ↑ → ↔ ↕ ⇄ ⇅ ⇆ ⥯ ⇌ ⇐ ⇑ ⇒ ⇓ ⇇ ⇊ ⇈ ⇉ ⮃ ⮄ ⮅ ⮆ ⮇
󱞭 󱞯 󱞱 󱞳 󱞵 󱞷 󱞹 󱞻 󱞡 󱞣 󱞥 󱞧 󱞩 󱞫 󱞽 ↳ ↰ ↱ ↲ ↴ ↖ ↗ ↘ ↙ ⇢ ⇠ ⇡ ⇣
⤓ ⤒ ↥ ↧ ⇤ ⇥ ⭰ ⭲ ↤ ↦ ⭾ ⭿ ↢ ↣ 󰕍 󰓦 ↪ ↩ 󰹹 󰹳
… × √ ÷ ° ∞ ≠ ≤ ≥ ≈ ± ␣ ¬ ‸ ™ ℠ ℃ ℉ № ℗ © ® § ¶ 󰟢 ¡ ¿ ⸮ ‽ ⁇ ⁈ ⁉ π μ ω Σ Ψ Ω
€ ¢ ₽ £ ¥ ₹ ₿ ₤ ¤
¹ ² ³ ⁴ ⁵ ⁶ ⁷ ⁸ ⁹ ⁰ ⁻ ⁺ ⁼ ⁽ ⁾
₁ ₂ ₃ ₄ ₅ ₆ ₇ ₈ ₉ ₀ ₋ ₊ ₌ ₍ ₎
ª º ᵃ ᵇ ᶜ ⁿ ⁱ ₐ ₑ ₒ ₓ ₔ ₕ ₖ ₗ ₘ ₙ ₚ ₛ ₜ
① ② ③ ④ ⑤ ⑥ ⑦ ⑧ ⑨ ⓪ ➉ ⑪ ⑫ ⑬ ⑭ ⑮ ❶ ❷ ❸ ❹ ❺ ❻ ❼ ❽ ❾ ❿ ⓐ ⓑ ⓒ Ⓐ Ⓑ Ⓒ
🯰 🯱 🯲 🯳 🯴 🯵 🯶 🯷 🯸 🯹 ½ ⅓ ⅔ ¼ ¾ ⅛ ⅜ ⅝ ⅞
␈ ␡ ␉ ␊ ␍ ␠ ⌘ ⌥ ⇧ ⌃ ⎋ ⏎ ⌫ ⌦ ⎇
▲ △ ▴ ▶ ▷ ▸ ▹ ▼ ▽ ▾ ◀ ◁ ◂ ◃ ⮜ ⮝ ⮞ ⮟ ⯇ ⯈
❮ ❯ ⟨ ⟩ ‹ › ‹ › ≺ ≻ ≼ ≫ ≪ ⫸ ⫷ ⚞ ⚟ ⟪ ⟫ ⟦ ⟧ ⟬ ⟭ ❨ ❩ ❪ ❫ ⧼ ⧽ ⎢ ⎥ ⌈ ⌉ ⌊ ⌋ ⎣ ⎦
∈ ∉ ∋ ⊂ ⊃ ⊆ ⊇ ∩ ∪ ⋂ ⋃ ∫ ∬ ⊰ ⊱ ⥾ ⥿ ⥼ ⥽ ⊻ ⊼ ⊽ ⋋ ⋌ ⋖ ⋗ ⟢ ⟣ ⟤ ⟥ ∴ ∵ ≔ ≕ ⋯ ‥ ⋮ ⋱ 
“ ” ‟ „ ‚ ‘ ’ ‛〝 〞» « ∗ ⋕ ❛ ❜ ❝ ❞ ′ ″ ‴     󰆕
✱ ✲ ✳ ✶ ✻ ✽ ❤ ♥ ♦ ♣ ♠ ♪ ♫ ⏴ ⏵ ⏶ ⏷ ⏸ ⏹ ⏺ ⏻ ⏼ ⏾
   󱂞 󰛢   󰼁 󰼂 󰟫 † ‡  󰗧 󰇀 󰈙 󰏢 󰅏 󰷴 󰫠 󱄏 󰶵 󰁳  󰥟 󱕅 󰻖 󰹧 󰸌 󱇪 󰌪 󰈷   󰤇 󱁕 󰔱
    ● ○ 󰧞 • · ◦   ✕ ✖ ✗ ✘      󰅖 󰛉 󱎘 ✓ ✔     󰄬 󰡕 󰸞
󰇊 󰇋 󰇌 󰇍 󰇎 󰇏 ⚀ ⚁ ⚂ ⚃ ⚄ ⚅  󰡜  󰡘  󰡗  󰡙  󰡚  󰡛
     󰌵      󰋼 󰛿 󰅙         󰔑 󰔓
󰴓 󰲶 󱡄 󰙏 󰙈 󰴒 󰈋 󱨍 󰃕 󰃉    󰏪 󰏫 󰙒 󰛓 󰈊 󰽉 󰃣   󰷤 󰏮  󰁨
    󰘬  󱕂 󰊢  󰧼 󱌣 󰆐 󰑶    
󰆽    󰗹 󱐡       󱐍 󰯈 󰠳 󱁖 󰮭
 󱑽 󱦡 󰏇 󰢛 󱡷 󰜃 󱢺 󱡊 󱁇 󱍢 󱢇 󰟟 󰜗 󱙷 󰓙 󰒡
■ ▀ ▄ ▌ ▐ ░ ▒ ▓ ▔ ▕ ▏ ▎ ▍ ▋ ▊ ▉ █ ▁ ▂ ▃ ▅ ▆ ▇ ▖ ▗ ▘ ▙ ▚ ▛ ▜ ▝ ▞ ▟ 🬀 🬁 🬂 🬃 🬄 🬅 🬆 🬇 🬈 🬉 🬊 🬋 🭨 🭩 🭪 🭫 🭬 🭭
⠷ ⠯ ⠟ ⠻ ⠽ ⠾ ⠁ ⠃ ⠇ ⡇ ⣇ ⣧ ⣷ ⣿ ⠿ ⠶ ⠤ ⠒ ⢀ ⡀ ⠈ ⠐ ⠠ ⢄ ⡠
┣┳┫┻┃┏━┓┗┛╋ ├┬┤┴│┌─┐└┘┼
```

The idea here is that you'll copypaste this into your terminal and will be able to see everything as normal — realistically a lot of these characters won't display for you *here*.
I replicate the array of my favorite fonts into the css of the website, but I only *vendor* Inter and Google Sans Code to you.
So neatly, if you have JetBrainsMono Nerd Font or Symbols Nerd Font Mono installed, you'll be able to view the above here in your browser :3 for whatever benefit that may serve.

# browsers aside

So far my configured fonts Just Work everywhere that I've tried, but *browsers* are the only spot where it's not *quite* 100% support.
The *array* of fonts is kept intact as it should, but the exclusion / inclusion of individual characters doesn't seem to work in the browser :< \
So, keep that tradeoff in mind as you go on to your font configuring adventure! 😌 \
Personally the nichest symbols I *mostly* see, and see *more* of, are in my **terminal**, so the mismatch doesn't bother me *that* much.

# footnotes

{{<bl id="1" />}} Noto stands for **no** **to**fu, where “tofu” is the name for that rectangle you see when some character isn't rendered by *anything*. And so, the goal of the Noto font is to make those basically *impossible*: supporting as many characters as there are ideally.

{{<bl id="2" />}} Adwaita is Gnome's fork of Inter; but there is no Inter Mono.
Adwaita Mono takes Iosevka as a starting point and tries to make it look similar to Inter.
Iosevka's niche symbol coverage is pretty respectable, so that's how I got a bunch of nicer renderings for free :D
