---
title: how to anki
date: 2024-07-05
draft: true
---

Anki is honestly a very hard to approach program. If you feel like it isn't, you're using it wrong /hj

In this post, I'm assuming that you already want anki and know *why* you want anki, so I won't be inspiring you; just sharing what I think is the optimal way to use anki.

In the next few sections related to settings, here's what you can expect: \
If I don't mention a setting, it's either insignificant or personal choice; you should check it out for yourself regardless. \
It's also possible you're reading this in the future, where a new setting appeared that I don't go over (my anki version is 24.06.3).

Settings that I *recommend* a certain value of are going to be marked by ❗ \
Settings that I VERY STRONGLY implore you to set to a certain value will be marked by ‼️

I'll be expecting you to have already installed anki; the pc version (which desktop os you have shouldn't matter, though).

An extra resource along with this blog post that I can recommend, is this [youtube video](https://www.youtube.com/watch?v=Eo1HbXEiJxo).

# Global settings

Let's go to global settings by pressing <kbd>ctrl+p</kbd>.

## Review

“Next day starts at”

The rule for the best time is to make sure it's impossible for you to see the next day's cards in the previous day, basically. My default is 5 am.

“Learn ahead limit”

‼️ 0 \
The whole point of the anki algorithm is to give you cards at appropriate times. Don't get into the habit of rushing, it is much much much more fruitful to instead get into the habit of checking back into anki a couple of times a day, where all the reviews have collected.

“Show remaining card count”

should be set to true \
because it's helpful to see whether you are currently on a new / learning / mature card.

“Show next review time above answer buttons”

I had this set to true for the longest time, but recently I disabled it! If you see that pressing “Easy” will make you see the card in 2 years rather than 1.5 years, you will be less likely to actually press it, even if it's the correct decision. Similar case for “Hard”. \
If you never see the time periods, you make answering decisions that are less biased.

“Spacebar (or enter) also answers card”

yep.

Set yourself some answer keys as well. \
Anki is more keyboard-supporting than you'd think!

# A bit of keyboard navigation

We're done in global settings.

Time to create and configure a deck :>

There's a Default deck by ...uhhhhhm... default.. Let's configure that one!

You could press on the cog, but I'll teach you to do that with your keyboard.

You can press <kbd>/</kbd> to search for a deck to study. \
In this menu, you can press <kbd>ctrl+n</kbd> to create a new one (don't do it yet). \
Currently you only have `Default` so you could just press <kbd>enter</kbd> immediately to pick it.

This moves you into the screen of your deck. No clue what it's actually called, I'll be calling it "yipee screen" from now on. Because after you finish your cards, it says “Congratulations!” :3

If you wanted to go back to the main screen of anki, you can press <kbd>d</kbd>. \
You might be quick to now search for `Default` using <kbd>/</kbd> again, but there's no need: the `Default` deck is already selected. \
It's going to be more obvious once you have more decks (if there is even a need), but there's a semantic of "the selected deck" in anki. You can press <kbd>s</kbd> to jump into the yipee screen of your selected deck.

<kbd>s</kbd> then <kbd>d</kbd> then <kbd>s</kbd> then <kbd>d</kbd> then <kbd>s</kbd>...

Alrightie now from the yipee screen we can press <kbd>o</kbd> to open the *deck*'s settings.

# Deck settings

I'll warn you immediately: if you press <kbd>Escape</kbd> from this point, it just discards your changes. You specifically need to press “Save” to save your deck's settings.

## Daily limits

### New cards/day

You might be quick to set this to a pretty high value. \
You are motivated and excited about anki, you're even reading a blog about it! You might've even watched a whole ass hour long youtube video about it. “Surely I can set it to like 50, right?”

WRONG. You have no idea just how strongly seemingly small effort compiles. You may be absolutely fine with 50 new cards one day, maybe even a whole week, but you're not using anki for a week's worth of memory, are you? \
Anki is *made* and *designed* to be *very* longform. "Your entire life" type deal. \
If you rush to set your new cards to some big amount, there's a good chance you'll burn out in like a month and maybe skip a day or two, then come back to 500 reviews and fully quit.

Anki is minmaxed with the assumption that you *will* **always** be doing your daily reviews. Maximize for *that* as well. For *consistency*, **not** burst.
I consider 30 new cards per day huge, but it super depends on the content and the time and effort you're willing to spend.

I use anki for:
1. morse code
2. hiragana
3. katakana
4. all country flags
5. all country locations on a map
6. programming languages
7. APIs
8. hotkeys; global ones and those local to specific programs
9. ascii decimal and hex codes
10. alphabet indexes (d = 4 for example)
11. I shit you not, my credit card information. Don't do this, anki isn't meant to securely store your credit card information, lol.

I, on average, spend 20 minutes per day on reviews, with ~11 new cards per day. \
This is quite small workload all things considered, but the massive benefit I get is that I'm excited about doing anki every day, and it's practically impossible for me to burn out on it.

So for the recommendation: you won't really know how many is too many, as it's likely you want to use anki for a different type of information than me, so set it to a reasonable amount (like 10), and feel free to change it as you continue to use anki. \
If you ever notice yourself feel burnt out with anki, **the** thing you should configure is the amount of daily new cards.

### Maximum reviews/day

‼️9999 \
Out of every single setting in anki, this one is my strongest recommendation. Hell, this is not even a recommendation — I ***beg*** you to set it to 9999 and never even consider touching it. \
If you ever *actually* get to 9999 daily reviews, you're fucked because you didn't listen to my advice about new cards per day above; the reason to set *this* setting to such a high amount is mostly semantic. \
Anki's algorithm is written in such a way where it's most efficient when it's allowed to give you cards at the precise intervals it has calculated. If it's arbitrarily restricted by a cap of reviews, it's simply going to spend more of your time and energy.

## New cards

### Learning steps

Because of FSRS (we'll get to it), this should only ever be a single step. \
When you press “Again” on a card, *this* is the time after which you'll see it.
I have it set to `2h`, you may choose `30m` or whatever else makes the most sense for your content. \
❗Do not set multiple steps, just one.

### Insertion order

Sequential (oldest cards first).

## Lapses

### Relearning steps

Same as [above](#learning-steps), but for mature cards.

### Leech action

‼️Tag only. \
I don't get why “Suspend card” is even an option. “If the card is too hard, stop learning it” — ?????????? wtf

## Display order

Here most things *can* work well; it depends on what you actually want. So for the most part you should click on each option title to read what it means in the built-in docs.

### New card gather order

Say you have overall 100 new cards; cards you have not yet reviewed. You "take in" 10 new cards per day. This setting decides *which* 10 cards to pick out of the 100.

The 2 values you should consider is “Ascending position” and “Random cards”. \
The former makes it so you go through new cards in the order that you added them (oldest first). \
The latter just gives you random ones.

Both are good, I just want to say that “Ascending position” is better than you expect. I personally use it.

### New card sort order

Sorts those 10 new cards you got after gathering them. Unless you've made a bad decision and there are 100 of them, this option doesn't really matter.

### New/review order

When you have both new (blue) and not new (red and green (learning and mature)) cards to review, which to show first.

“Show before reviews” ensures that you *always* take in all the new cards first, and then all the reviews. \
I use it, but I don't recommend it — it's a risky choice. It is better to force yourself to go through all reviews first, before new cards (with “Show after reviews”). However, if you are *sure* you will complete *all* of your reviews, starting with the new cards is very exciting!

“Mix with reviews” is quite reasonable. Start with it if you're not sure yet.

### Interday learning/review order

Show before reviews.

### Review sort order

There's quite a lot of options here, here are those that I would recommend:

1. Descending difficulty
2. Random

Yep, just two! :3

## Burying

Enable everything.

## FSRS

Now the JUICE of the settings!! Or rather, of anki itself! FSRS!

This is the cool new algorithm that you should be using! Enable it.

### Desired retention

Don't touch it for now. Come back in at least a month. I am serious.

If you read the following before having done anki for at least a month, you instantly die: you may increase or even decrease this depending on how strongly (or weakly) you want this deck to be remembered.

Alright, the 10% of readers who survived, let's continue!

You should click the “Optimize” button once every ~1-3 months, *per* deck. Don't forget to save right after, too!

‼️Do **NOT** press “Reschedule cards on change”. You may be excited to put in some extra work to make sure your cards have the perfect periods, but the tradeoff ends up making things worse.

There's a good chance you will instantly get a **bunch** of extra reviews at once, which you may not be able to do all at once. So their "perfect periods" won't even be perfect.

But that's misleading too: you *shouldn't* do them all at once. Because if one day has a bunch of reviews at once, that will repeat on another day some time later. \
Your reviews that got naturally padded out throughout the days to be balanced, will *no longer* be balanced. \
So pressing “Reschedule cards on change” puts you between a rock and a hard place, it's simply not worth it.

## Advanced

### Maximum interval

‼️DON'T set it to a reasonable value. I have it set to 100 years (36500). \
Trust the anki algorithm.

---

Cool we're done configuring the deck!

You may want to create multiple decks for ...
