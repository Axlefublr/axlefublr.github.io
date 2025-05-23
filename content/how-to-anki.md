+++
title = 'how to anki'
date = '2025-01-07'
+++

Anki is honestly a very hard to approach program. If you feel like it isn't, you're using it wrong /hj

In this post, I'm assuming that you already want anki and know *why* you want anki, so I won't be inspiring you; just sharing what I think is the optimal way to use anki.

In the next few sections related to settings, here's what you can expect: \
If I don't mention a setting, it's either insignificant or personal choice; you should check it out for yourself regardless. \
It's also possible you're reading this in the future, where a new setting appeared that I don't go over (my anki version is 24.06.3).

Settings that I strongly *recommend* a certain value of are going to be marked by ❗ \
Settings that I VERY STRONGLY beg you to set to a certain value will be marked by ‼️

I'll be expecting you to have already installed anki; the pc version (which desktop os you have shouldn't matter, though).

An extra resource along with this blog post that I can recommend, is this [youtube video](https://www.youtube.com/watch?v=Eo1HbXEiJxo).

# Global settings

Let's go to global settings by pressing <kbd>ctrl+p</kbd>.

## Review

### Next day starts at

The rule for the best time is to make sure it's impossible for you to see the next day's cards in the previous day, basically. My default is 5 am.

### Learn ahead limit

0

This setting is very weird.

> TLDR: don't use it, but if you're still interested, continue reading this section

Imagine you have a bunch of reviews (green (mature) cards) left to do in the deck;
when you press “Again”, you see the card in 20 minutes.

As you go through your reviews, you might take so long so that you see the “Again” card in the same review session. \
Usually you're supposed to set [learning steps](#learning-steps) to a high enough value where that won't happen.

So, you go through all of your reviews, and there are 10 more minutes until you see that “Again” card. \
There is *nothing* left for you to review; you can leave.

What “Learn ahead limit” lets you do, is cut down those 10 (or however many) minutes down to zero. \
It starts acting once you *finished all reviews*.

This is different from the default behavior of learning cards (red ones): those usually can appear whenever in your review session, not just after all other ones are finished.

“How is this useful?” you may ask; and my answer is *exactly*. It is somewhat useless in my experience.

### Show remaining card count

Should be set to true

Because it's helpful to see whether you are currently on a new / learning / mature card.

### Show next review time above answer buttons

I had this set to true for the longest time, but recently I disabled it! \
If you see that pressing “Easy” will make you see the card in 2 years rather than 1.5 years, you will be less likely to actually press it, even if it's the correct decision. Similar case for “Hard”. \
If you never see the time periods, you make answering decisions that are less biased.

### Spacebar (or enter) also answers card

yep.

Set yourself some answer keys as well. \
Anki is more keyboard-supporting than you'd think!

# A bit of keyboard navigation

We are now done in global settings.

To discover a bit of the keyboard navigation support anki provides, let's move onto creating and configuring a deck.

There's a Default deck by ...uhhhhhm... default.. Let's configure that one!

You could press on the cog with the mouse, but I'll teach you to do that with your keyboard.

You can press <kbd>/</kbd> to search for a deck to study. \
In this menu, you can press <kbd>ctrl+n</kbd> to create a new one (don't do it yet). \
Currently you only have `Default` so you could just press <kbd>enter</kbd> immediately to pick it.

This moves you into the screen of your deck. No clue what it's actually called, I'll be calling it "yipee screen" from now on. Because after you finish your cards, it says “Congratulations!” :3

In this screen, if you had any reviews to do, you would be able to press <kbd>s</kbd> to start the reviews. But that's for later; you don't have any cards yet.

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

### Leech threshold

At least 3, but then it depends. Personally I use 5.

The concept of leeching is made so that you can notice which cards you keep pressing “Again” on *repeatedly*.
So, once you press “Again” on the same card n times in a row, it gets the `leech` tag.

This by itself does absolutely nothing: it's up to you to decide what to do with that information.
It's your wakeup call basically, that "hey! this card is really difficult! try coming up with a mnemonic for it maybe?".

Once one of my cards becomes a leech, I take some extra time to really burn it into my brain, and press <kbd>ctrl+alt+n</kbd> to reset it back into a new card, so that I can relearn it again, without the ease factor being fucked up.

With FSRS, it's probably a bad idea to reset it like that, so I don't necessarily recommend it. Just, make sure to pay extra attention to the card.

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

It's been more than 2 months after I pressed Reschedule, I'm still eating shit to this day. Don't repeat my mistake.

## Advanced

### Maximum interval

‼️DON'T set it to a reasonable value. I have it set to 100 years (36500). \
Trust the anki algorithm.

# Deck meta

We're done configuring the deck!

You decided to start using anki, most likely, because you already have one or multiple things that you want to learn.

It is natural now to create a deck per each subject that you want to learn.

I've been using anki for around two years, and I found that anki decks have actually quite poor UX.

Let's pick a list of subjects out of those that *I* personally learn, and split them into decks.

1. morse code
2. ascii characters' decimal codes (from a space to `~`, which is from index 32 to index 126)
3. all countries on a map
4. all country flags
5. useful flags of terminal programs
6. the standard library of the programming language rust

## Deck splitting

The obvious solution here is to create 6 decks. I only have 2.

Morse code is around 50 cards, ascii characters is 94, both country flags and map locations are around 250.

What unites all of them is that they're *finite*. \
You will eventually go through all of those cards; you won't be adding more.

After a while, you'll need to review them less and less. The decks of each one of them will become a “dead deck”: a deck with only a few reviews to do, if any, that you *still* have to check, travel to, open, review, and go back from.

Anki can take a lot of energy, and if you grow to say, 13 decks (I used to have around that many), you will be spending a lot of extra effort on *just* moving around. \
A lot of times, you don't *just* review the mature cards — you'll fail some of them, and have to come back to re-review them. \
That is a lot of travel time!

## Decks as organisation

You may think that this is an okay tradeoff for the *organisation* that decks provide. \
But actually, decks are shit at organisation. I'll expand on the *solution* in the [note types](#note-types) section, but for now I'll elaborate on why decks are bad at organisation.

Morse code and ascii indexes. \
How would you structure the question->answer in those?

What I find to be most effective is *just* the symbol as the question, and *just* the sequence / number as the answer. \
This way, I don't spend extra energy on parsing useless words like "what is the x character in morse code?", and instead *just* focus on the question at hand. \
Getting to the *core*, the *meat* of the question, without all the extra fluff, that *really* piles up, believe it or not!

I'll expand on card creation in [this upcoming section](#card-creation), but for now let's stop at the idea that cards should be very minimal.

{{ hr(id="minimal") }}

Great! We put ascii cards into the “Ascii” deck and morse cards into the “Morse” deck.

As you go through your thirteen billion decks, you *will* find yourself confused.

You open your morse code deck to do the measly two reviews, and in the rush of answering cards, seeing `x`, you answer `120`, rather than —・・—

This happens because anki does not display the title of the current deck you're studying. There is no looming *context* for you to be able to check back with, and so you'll find yourself answering the *wrong question* a lot of the times. \
You answered correctly, but to the wrong question.

What do you do in this situation? You eat sand.

You have to introduce context somehow, to not confuse cards with similarly-sounding questions. \
And this is why the first jump is the "What is x in morse code" shenanigans.

Decks don't solve organisation, so believe it or not, I'll recommend putting both ascii and morse code into a *single* deck. We will solve the confusion in the [note types] section.

## When should you deck?

There are only a couple of benefits to creating a new deck. If you don't meet any of them, I don't recommend creating a new deck.

### News splitting

We just added, like, 500 or so new cards. These are long-term — you aren't meant to intake them all at once. So, we need to come up with a scheme to split those new cards per day in a reasonable fashion.

Say you added morse code first, then ascii indexes, then flags, then map locations.

Depending on your [new card gather order](#new-card-gather-order), you may either go through all of them in order, or through all of them randomly.

If that's exactly what you want, blammo them all into a single deck, unless the next sections on decking positives apply.

But let's say you want to be learning three categories at once, rather than trying to rush a single category / randomly picking cards from all *four* categories.

You feel you have 3 new cards per day in you¹, and you want 1 of them to always be morse, and 2 of them to be random selections from the country flag + map location "pool".

> ¹ pure example, I don't actually think this is viable for the categories at hand

To express something like this, you *need* to split into two decks.

{{ hr(id="two-decks") }}

If you were okay with just doing the categories in order, with all 3 new cards being for the same category, you wouldn't need two decks! \
The “Reposition” action in the card browser (<kbd>ctrl+shift+s</kbd>) would allow you to put all the categories' cards in the order that you want, even if you *created* them all in the wrong order¹.

> ¹ technically speaking you can avoid having to create another deck using the Reposition action by manually positioning each and every card, but this is herculean effort compared to just creating a new deck, and then moving its cards into another deck, once you go through all of the new cards

But in our case, we create a deck for Morse¹ code: it will give you 1 new card per day; \
And a Country¹ deck that will hold country *flags* and country *locations*. Its [new card gather order](#new-card-gather-order) setting will be random.

This way, you guarantee 1 new card of morse per day, and 2 "country-related" cards per day, totalling three. All this without touching the ascii indexes for now — you'll get to them once you fully learn one of the three categories.

> ¹ names are arbitrary, choose whatever you wish

### Ordering

We touched on the [new card gather order](#new-card-gather-order) setting — it's quite an important one!

For some type of cards, it makes the most sense to learn them in random order. By "fuzzing" them like this, you avoid the issue of one card half-answering the question of the next one.

For others, where that half-answering issue doesn't arise, it is best to go with sequential. \
If you keep adding cards while being served them randomly, you'll end up having "unlearned spots" — some cards will slip through the cracks of how randomisation inherently works, and you might take too long to get to some crucial information to learn. \
If you learn sequentially, you ensure that you finish a given subject before going onto the next one.

### Optimization

FSRS optimization happens *per deck*. More precisely, per deck *preset*.

It does its magic depending on how you answer the cards in the given deck. I don't know exactly how it works, but the main idea is this: vastly different subjects will be optimized differently.

Both morse code and ascii indexes are fairly similar categories, that I'd argue are very similarly difficult to *memorize* and to *remember*. \
Splitting them up into two decks just for the optimization I don't think is worthwhile — you will likely respond very similarly to both, and so they could just be kept in a single deck, optimized together.

FSRS optimization makes it so you see the cards at the correcter time periods, to achieve your desired retention rate.

When the information is similar enough, you'll lose far more time, effort, and motivation by switching a billion decks, than you would *gain* from slightly correcter periods.

After switching to FSRS, for *four months* I didn't even know I could "optimize parameters" in FSRS, much less that I should be doing it monthly. And yet, I was perfectly happy with how anki was working for me. So genuinely don't worry too much about this.

To be fair, what counts as "similar enough" is very personal, I don't know if a hard rule could be worded to help you decide. So it's not unreasonable to start with making more decks than needed, and later on merging some (if not all) together, once you get a feel for anki.

### That's it!

Not many reasons to make extra decks, as you can see. I hope you can keep these in mind to help you not fall into the "trap" of the supposed deck meta, winning you time, and saving you effort.

Now to the *really* fun part >:3c

# Note Types

You know that gif with Tony Stark dramatically hitting a helmet with a hammer? \
This is me with note types.

I have tried multiple approaches in using them the most effectively, and found one I believe to be perfect.

Let's start with the basics.

## Default note types

In the main screen, press <kbd>ctrl+shift+n</kbd> to go to the list of default note types.

1. Basic
2. Basic (and reversed card)
3. Basic (optional reversed card)
4. Basic (type in the answer)
5. Cloze
6. Image occlusion

Some of them are quite a mouthful. I'll refer to them as this:

1. Basic
2. Double basic
3. Basic?
4. Type
5. Cloze
6. Image occlusion

If you want, you can even rename them!

Note types are going to be something you'll keep on selecting, so having them be unique and easily typable / searchable is very helpful. \
My “Basic” is just `d`, and “Cloze” is just `h`.

The default note types showcase what anki even *allows* you to do in the first place, in terms of question->answer organization.

A note type is a template, a constructor: you define how one creates *cards*. \
You review cards created using notes. \
A single note can (in theory) create an infinite amount of cards.

### Basic

Is the most *basic* note type: it creates a single card.

### Double basic

Is like the above one, but here the magic starts: it creates a card for question->answer AND for answer->question.

It is a note type that creates TWO cards, one for each "direction".

### Basic?

You can probably guess now that you can use “Basic?” to *optionally* create the answer->question card, while it doesn't do so by default.

### Type

Lets you type in the answer, rather than just recalling it in your head.

### Cloze

Omits some part, or *multiple* parts of the question, and you need to mentally *fill in* that missing part, to answer the question.

### Image occlusion

Hides some part of an *image*, that you need to mentally *flll in*. In other words, it's “Cloze”, but for images.

## Creating your own

Defaults are made to be configured away, so let's discover how you can create your *own* note types.

In the <kbd>ctrl+shift+n</kbd> screen, click on Basic and then Fields.

### Fields

A field is an inputbox, the text of which is used in some way in the note type. \
Here in Basic, you have 2: Front and Back.

You can add as many extra fields as you want (at least I don't know of a limit), however they won't *do* anything by default. We'll get to how to make their text appear in your card later. \
For now, know that you can add multiple fields to a given note (including your own ones we'll create later), and have them appear in the card in different ways.

Anki supports html: when adding a card, you can press <kbd>ctrl+shift+x</kbd> to toggle the html editor. \
But if you *always* want to edit a given field as html, here in the Fields view you can set the option below to make the field “Use HTML editor by default”. \
On a non-html field, the <kbd>ctrl+shift+x</kbd> hotkey then toggles on the non-html editor.

Go back to the previous screen by pressing <kbd>escape</kbd> and then click on “Cards”.

### Cards

This screen has been confusing for a while to me. So if it feels intimidating, don't worry — it *is*.

The source of this intimidation for me is a couple of buttons and popups that we'll get to at the end of this section, for now let's explore the more obvious parts.

Note types are templates that decide how to create cards. Each note type can create as many cards as you want it to. \
Although by default, a note type creates just one card.

#### Front Template

On the left side, you can see an edit field, with three radio buttons above. \
Here, you define the html template for the cards to be creates with this note type.

The main juice that actually makes note types useful at all, is `{{Front}}`. \
`{{Front}}` is a template (that I'll be calling a shortcode) that is replaced with the resulting html that you put in a field.

Notice the name: `Front`. This is a name of one of the fields of this note type. \
In the previous section, I mentioned that fields aren't used automatically. \
*We* get to decide where and how they show up, by putting them in explicitly.

Say we created a field called `Extra`. We can now add `{{Extra}}` somewhere in the template for the note type, and the text (resulting html) that we put in this `Extra` field is going to show up there!

#### Back Template

Now, click on the radio button “Back Template”. The html is a bit different now!

The real semantic of the first and second templates is the following:

1. This is how the card looks when I'm presented the question, and haven't yet answered the card.
2. This is how the card looks when I answered the question, and pressed space to see the answer.

By default, the Front side of the card is also included in the Back side, using the special shortcode `{{FrontSide}}`. \
It takes the entire Front side html, and blammoes it in the Back side template.

There's nothing stopping you from removing it, though. You can simply remove it from the Back side template, and you won't see the front of the card after pressing space. \
I don't recommend doing so; you would be shooting yourself in the foot for no benefit. \
However I hope that showcases that templating is not that magical! \
How anki shows itself to you is defined clearly, and is changeable by you.

#### Styling

Probably more changeable than you expect!

The third radio button is my favorite one: you get to define the CSS of your note type! \
Yes, you can make it look however you wish!

I used this to change the fonts used, the colors, and added inline and blockwise codeblocks.

You might reasonably ask how you can “Inspect Element” anki to figure out how to css it correctly. \
There is some plugin for it, and likely some people in the forums have figured out most things.

However, I just guessed my way throgh most of it, so I won't give precise advice that I don't have checked. With styling selectors you need, you're on your own.

#### Card Type

Let's look at the popup that scared me at first. Well, even now I never touch it (I'll explain why later) \
At the top, you can see “Card Type” with something like `1: Card 1: Front -> Back` \
Let's process that.

A note type can create any number of cards. That is what the `1:` stands for. \
Which one? I have no clue. At least one of them, lol. The other one of them stands for god knows what.

So, `1: Card 1: Front -> Back` means: out of potentially many, this is the first card that's created by this note type. In this specific card, the Front side is the question, and the Back side is the answer.

Inconveniently, the fields are called the same way as another concept, making things less clear.

Anki has a concept of "front side" and "back side". We glanced over that in the first two radio buttons. \
So here, the front *template* is used for the front side. The back template is used for the back side. \
As expected.

However if we looked at “Basic and reverse”, the second card that the note type makes says `Back -> Front`. \
So, the *back* template is used for the front side (the question) of the card, and the *front* template is used for the back side (the answer) of the card.

That's how that note type achieves the "and reverse" part — it creates two cards — one is front (question) to back (answer), and another is back (question) to front (answer).

Cool, isn't it? \
However, as I mentioned, I won't be teaching you how to make use of this, because I don't use it.

For the most part, adding two cards per each note, just to learn the question from both sides, is a waste of effort, in my experience. \
I used to learn languages using anki in the past, and there the reverse thingy *was* helpful, but for what I use anki right now, it's fairly useless.

{{ hr(id="note-type-meta") }}

Viola! Now you have the necessary knowledge to learn about the card organizing meta that I've made for myself. I hope you'll like it as much as I do!

Rather than using a bunch of different decks (as I mentioned, I only have 2), or a bunch of different note types per subject (used to do that, very laborious), \
I only have a couple of note types.

Each of those note types includes two extra fields: one is called “Header” and the other is called “Subheader”.

Because the anki browser allows you to search by each field if necessary (`header:ruby` for example), this works amazingly for card organization.

If I need to do bulk actions on a bunch of cards of a certain subject, I *can*!
Just as easily as I would with the many decks approach, and *more* easily than with the many note types approach.

In terms of the note types, I basically only have Basic and Cloze. Except, I split Basic into 4 different note types:
1. the normal Basic one
2. 1 but back side is a codeblock
3. 1 but front side is a codeblock
4. 1 but both sides are codeblocks

I think this is niche? But I use it so much that it made sense to make a shortcut like this for myself, rather than always having to wrap things in a codeblock in the html of the card manually.

The approach is pretty simple: you come up with an element (I use `<cd></cd>`) and make css for that element to display it as a codeblock. \
The note type then wraps `{{Front}}` in that element like `<cd>{{Front}}</cd>`. Blammo.

I will link to my html templates and css file, but be warned: I made this specifically for myself so I give no guarantees it will be a good starting point.

[html templates main link](https://github.com/Axlefublr/dotfiles/blob/main/anki/card.html) \
[html templates backup link](https://github.com/Axlefublr/dotfiles/blob/d21744e691c09e9980fd01b013c0daaf64a2234c/anki/card.html) \
[css main link](https://github.com/Axlefublr/dotfiles/blob/main/anki/card.css) \
[css backup link](https://github.com/Axlefublr/dotfiles/blob/d21744e691c09e9980fd01b013c0daaf64a2234c/anki/card.css)

As I alluded to prevously, this approach also solves another issue: I am never confused as to what subject I'm currently studying. It's right there at the top! Decks simply don't do this.

So, I get to blammo all the cards in 1-2 decks, and go through them all at once; I don't have to experience the annoyance of jumping between 15 or so decks.

# Card Creation

First of all, [read this](https://www.supermemo.com/en/blog/twenty-rules-of-formulating-knowledge). \
This blog post has massively helped me create better and more efficient cards.

I won't attempt to tldr it, but here are some things that I ended up implementing into how I make cards.

## Why use many word when few word do trick

"What was the year at which Marie Currie did x?"

Is very wordy. I'm exagerating in this example, but even still you'll find yourself making cards that use many extra useless words, kinda like the sentence above.

Ideally, you will be doing anki until you die. That is a long ass time! (Hopefully?) \
So all the time you spend parsing words that don't help you arrive to the *core* of the question, is time wasted.

It's not like reading a novel: *there* reading 20 useless words in a row is fun (allegedly).
*Anki* you are doing for an exact benefit. So cutting down on non-core words is actually crucial.

"Marie Currie did x when"

That ↑ is all you need. Even the `?` at the end is bloat. You don't need it.

Your target should be to minimize the amount of time it takes you to process the question. Sometimes adding an extra word can actually *help*, because it looks more natural to you that way.

So, try different approaches for debloating cards, and see what works for you. You will notice just how much faster and easier you can do reviews, because there's much less for your brain to process.

You can then use this extra energy to do *more* cards, or to do the same amount of cards in less effort, to spend that effort elsewhere.

## Make use of headers

The note type approach I use helps the previous point.

"jj revset operator that gives you the intersection of two other revsets"

becomes:

```
jj
revset
intersection operator
```

Header `jj`, subheader `revset`, and less wordy question. The word `operator` there I'd remove. \
Headers and subheaders let you encode a part of your question into a section above the question, that you will be able to skip over most of the time.

If you read the question and you naturally remember that the question is about jj, you won't need to pay attention to the header *or* subheader.

At least for me, it's natural to read the question first, and so header + subheader lets you move out a part of the question that otherwise, you would have to read *every* time. Small optimization that sums up to more than you would expect.

## One thing per thing

Split the question into as many cards as you reasonably can. \
The blog post I linked above goes into good detail on what that entails, please read it.

If you make a question and a part of it could become a whole another question, then it *should*.
It might look kind of ridiculous, but the amount of time you spend is actually pretty big.

❌ question: "shell -> pueue group -> delete a group, moving all of its tasks to the default group" \
❌ answer: "pueue group remove"

This question is worded in such a way that you hope you'll remember some extra information in parallel. Memory doesn't work like that, you won't be able to "brush off" information in an effective manner. That part should just become its own question if you want to remember it, and be discarded if you don't.

✅ question 1: "shell -> pueue group -> delete group" \
✅ answer 1: "pueue group remove"

✅ question 2: "shell -> pueue group remove -> what happens to the tasks when you delete a group?" \
✅ answer 2: "they get moved to default group"

If each individual question is really easy to answer, all of them will quickly rank up in their intervals, and not take much of your time. So, prefer more cards to less cards, counter-intuitively.

If you make one complicated question, you will hit your head against it time and time again, and end up wasting a lot more time overall.

Similarly, if you expect yourself to give a big, multi-stage answer to a single question, you should also split it into multiple cards.

{{ hr(id="thanks") }}

Thanks for reading! This took me multiple months to write, lmao. Kinda burnt out on it; you might've noticed towards the end. Hope my experience will be useful to you!

# addendum

I'm writing this on 2025.05.19.

While most things hold up exactly as before, I'm now using a slightly different approach in terms of taking in new cards. \
Not entirely sure if it ends up being more or less efficient, but the consistency of it is awesome.

I learn for the love of the game, so there isn't necessarily some deadline up until when I have to learn everything I want to. \
What matters far more to me, is to ensure that I never burn out on anki.

I found that for my type of cards, I can review 70 every day. I can (and did, before finding this method) in theory do more, but I know that more than 70 will end up burning me out, if prolonged.

> This is a type of thing that you get a feel for after a while of using anki, so don't worry if you don't know your number yet.

I found a way to review exactly my limit of cards every day, without fucking up the algorithm too much!

First, a setting that this entire approach relies on: Deck Options -> Display Order -> New/review order -> Show after reviews. \
Now, when we start doing our reviews for the day, we go through all the red and all the green cards first. \
Say I have 15 reds, and 45 greens. That's 60 total reviews to go through. I got 10 extra in me though! And those 10 extra are for the new cards :D

So, as I review, I check every now and again:
1. how many reviews (not new cards) are left
2. how many reviews I've done so far

After I get through all the reds and all the greens (unless they are already more than 70, I elaborate on this later), I check `70 - total_reviews` to figure out how many new cards to take in today. \
Your total reviews are shown on the main / home screen, so that's what I'm relying on. I wonder if there's a plugin that would show that info *while* I'm reviewing! \
For easier math, I set the amount of daily new cards to 100. For reasons I explained in this blog post above, *obviously* we aren't meant to gulp all that 100. We simply take from that 100 every day, however much we can handle (`70 - total_reviews`).

Why this appproach is amazing, is that it's quite dynamic while being consistent in terms of my effort. You have a low amount of reviews? Yipeee you get to get more new cards today! Too many reviews? No big deal, we simply take less / no new cards.

But what happens if you have more than 70 reviews? This happens to me every time I rotate my sleep schedule, for example. Or say you miss a day for other reasons.

Generally, it's a bad idea to gulp all the reviews at once, that you got from the previous day that you skipped. \
Sure, the anki algorithm works better this way, but that's only in theory. In practice you are human. \
Additionally, you're creating a fucky effect: if you get a sudden spike of reviews on a given day, this spike will repeat some days later (although it won't be as strong).

This is why instead of gulping all reviews in a bad situation like this, I tend to instead increase my daily reviews temporarily, until the extra cards are all sorted out. This happens over the course of multiple days, and so the spike repeat is not as big of an issue. \
Deck Options -> Display Order -> Review Sort Order -> Ascending retrievability is quite important to make this as effective as it can be, to not fuck with the anki algorithm as much.

So! If you take the same strategy, my "up to 70 reviews" approach makes even more sense. In the general case, my reds + greens are less than 70, and I fill the rest with new cards. So when shit hits the fan, what ends up happening is that reds + greens fill up the entire 70, and I simply don't take new cards for a couple of days. So, this approach has built in overflow protection! Although it's not like you should want to use it :p \
To add, when I get an overflow, I increase my cap from 70 to 80, until it's sorted. I consider a shitfan sorted, when to get 70 reviews, I'd have to take new cards. At *that* point I go back to the 70 cap, and take as many new cards as fit into the 70 cap, rather than the otherwise increased 80.

In the case where I get more than 70 reds + greens, but there *isn't* an overflow from me skipping a day, the same rules apply, except I don't touch the cap. So, I basically allow the remaining reds + greens go to the next day, and let them get sorted out automatically via me taking in less new cards. Quite dynamic!

Aside from the overflow protection, what I love most about this approach, is that I never get "unlucky". \
I never get too many reviews on a day where I'm exhausted. \
I never get too little reviews on a day where I'm extra energetic. \
It's always the same exact number of reviews :D

And what's fun about this, is that it lets you more clearly tell what your cap is. If you do exactly 70 (or your other number) reviews *every single day*, you will realize sooner, whether it's too much or too little. \
My cap used to be 65, but over time I noticed that I wanted to do more, and so I increased my cap to 70. I want to increase it again, but I'm intentionally waiting for a while to make sure that would be the correct move. \
Don't rush when changing your cap, and change it by small amounts.
