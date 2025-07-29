+++
title = 'proxy things'
date = '2025-07-27'
+++

I'm unfortunately in russia. \
Since the start of the war, most services have left the country, now requiring a vpn to use.

But funny thing — so did the *vpns*!

First, some now required a vpn to access them (the another vpn). \
Sounds pretty useless, but I could conceivably use some bad free vpn to then enable the good one, disconnect the free one and continue using the good one until I reboot. \
So that worked to an extent, but it didn't last that long. \
One by one, vpns started massively throttling, taking tens of minutes to connect, and eventually just not connecting at all. \
Express, Nord, Air, Surfshark, CyberGhost, Private Internet Access, Proton, Windscribe, Mullvad — are all vpns I've used or tried using, until ultimately they ***all*** broke.

I'm in quite a salted vegetable here, I certainly need to access things like discord, youtube, spotify, and most of the internet honestly; what do I do?

# the name of this heading would be a spoiler to the dramatic tension of the previous sentence

VPNs are not the first technology that allows you to avoid geo-restrictions. \
Thanks to my friend's effortful history lesson on networking, I got a better idea of what a *proxy* is. \
Better enough to now consider it as a possible solution.

A proxy, in my understanding, is a server that can make requests *for you*. \
It's exactly like sexism actually:{{fn(i=1)}} you, the russian woman, saying something to another server is rejected. \
So you ask a man (the proxy) to say the same thing *for you*, and give you the response — now it suddenly works.

VPNs often work globally on your system which is very convenient,
but you do tend to *need* geo-restriction-unlocking for only a few places. \
Those few places, usually, allow you to configure the proxy!

There are HTTP proxies you can find out there for free, but really shouldn't:
the S part of HTTPS is very important for the *security* of your data, as without TLS,
the data you send will be visible to the proxy itself — a bank password that you enter, for example. \
Finding free HTTP*S* proxies is then a *privacy* concern, so I decided to *buy* one instead.

# for the russians

In this section I'll go over my exact process for buying a proxy, \
but if you're not russian you're better off using some store that's not specifically tailored to russians. \
Google for “buy https proxy” and you'll probably get there.

Although, I'd recommend actually going through with my suggestions in the blog post, after you've completed it — I will be omitting some things until later notice for dramatic effect.

As a hilarious way to force non-russian readers to skip this section, I'll write it *in* russian.

Нашла сайт, где можно купить прокси: <https://proxys.io/en> \
Когда юзала VPNы, самые лучшие результаты у меня были используя сервера в Нидердландах, поэтому решила купить и прокси там же.

Я плачу криптой, если честно уже не помню почему; \
Для этого я использую сервис [MATBEA](https://lk.matbea.com/en). \
У них ещё есть приложение на андроид, оно мне кажется более приветливым, так что обычно использую его.

С помощью MATBEA, можно обменяться валютой с человеком —
вы отправляете классические деньги со своего банка (тинькофф например{{fn(i=2)}}), \
они подтверждают что пришло, \
и крипта, которую MATBEA держит во время операции{{fn(i=3)}} приходит к вам на аккаунт.

Но на протяжении этой операции, есть несколько коммиссий из-за которых сложно предугадать сколько денег в итоге попадёт на счёт proxys.io. \
Так что моя личная стратегия, ложить на 30% больше денег на счёт матби, чем я собираюсь в итоге получить на счёт proxys.io. \
Не волнуйтесь, реальная коммиссия не буквально 30%, это больше для безопасности, чтобы потом в жопу не ебаться когда вдруг не хватает 0.003 биткоина. \
Как моя подруга сказала на этот счёт: “лучше перебдеть чем недобдеть”.

И так, план следующий:
1. Выберите https прокси которую хотите купить
2. Оплата через крипту
3. Вам выставится адрес, на который нужно отправить какое-то количество скорее всего биткоина
4. Идите на матби
5. Переводите чуть побольше чем надо на матби аккаунт с помощью “Пополнить”
6. Сделайте “Обмен” на валюту, которую proxys.io от вас хочет
7. Жмите “Отправить”, на адрес получателя
8. Восемь раз отмерьте (проверьте что правильно скопировали и вставили адрес, или просканиуйте qr-код)
9. Один раз отрежьте (отправляйте, но поставьте низкий приоритет чтобы не было ебейших комиссий)
10. Ждите миллиард лет, пока деньги дойдут до proxys.io. У меня это обычно занимает где-то час

Готово! Теперь у вас есть прокси 🥳 \
Юпииииии

Блять ненавижу писать по-русски, это намного сложнее чем по английски 😭 \
Но “бит” смешной так что надо было 😤

# по сусекам

I have an https proxy now! :D \
And I paid just 2.67$ (monthly) for it!

VPN prices generally *start* at 5$, so that's a pretty damn stellar deal,
considering how effective the proxy later became for me. \
So even if you aren't in my situation, and VPNs *do* work for you, using a proxy might still be more cost-effective, if nothing else!

{{hr(id="information")}}

The proxy comes with a set of “information” that you're going to use to plug it into places. \
Let's name all of them, so that they're a bit more familiar!{{fn(i=4)}}

|thingy|namey|
|------|-----|
|162.3.164.178|ip address / hostname|
|8676|port|
|user223767|username|
|eIwB7xB|password|

The ip address + port are probably discoverable enough{{fn(i=5)}} where someone unwanted could get access to your proxy, so it will usually come with a username and password as well. \
You are meant to keep these secret 😼

A lot of places that let you configure a proxy, simply have separate fields for each of the 4 values. \
But some don't!

The following syntax is something you might see yourself using in those other places:
```
user223767:eIwB7xB@162.3.164.178:8676
```
i.e.
```
username:password@ip:port
```

It's rare to use them bare like this though, usually you're expected to give a link:
```
https://user223767:eIwB7xB@162.3.164.178:8676
https://username:password@ip:port
```

# it works... sorta

The main, and almost *only* place where I need georestriction unlocking, is my browser. \
Through some steps (that I'll teach you later), I was successfully able to use my new proxy, \
and no longer needed to jump through 5 free vpns :D

It seemed surprisingly straightforward, and continued working for a while, until I switched to [floorp](/content/floorp/index.md). \
In firefox, everything worked just how you'd expect. \
But on floorp, suddenly, youtube videos were not playing 🤔

Note, the entire rest of the website was working, just not actual playback. \
At the time, this put me into anxiety mode; I was considoring{{fn(i=6)}} using firefox specifically for youtube, but that sounded like quite a shitty tradeoff. \
Firefox and floorp handle proxies differently *somehow*.
I wasn't quite sure why yet.

My favorite puppy{{fn(i=7)}} helped me solve this, with some bespoke setup that involved its VPS' socks4 proxy. \
Still not exactly sure what all this is, but now I'm hearing this 🧦 term when it comes to proxies. Interesting.

And so, I was paying for my own proxy to use for most things, and using my partner's socks4 proxy for youtube. \
Feels kinda bleghy, right? \
Shouldn't my own proxy already accomplish everything?

I realized my grave underunderstanding recently, while trying to set up discord. \
*Web* discord is just fine for me, but it's crucially missing the ctrl+123456789 hotkeys, so I really wanted to try out vesktop to get those. \
Both fortunately and unfortunately, *there* the proxy setup was more difficult to figure out. \
As I was trying different things, I came across a new nugget of knowlege: \
“https proxies are solid for TCP, but fucky for UDP. socks5 solves this though”

Through my friend's networking history lesson, I got a slightly better idea of what TCP and UDP are usually used for. \
And UDP, crucially, is used for *video playback*, among other things!

Huh! The exact thing that's *not* working off of my proxy in floorp. \
Hmmm… So if I had a socks5 proxy, this might get solved?

I was playing around in my proxys.io (where I bought my proxy) account, and happened to enter into the proxy's information page again. \
I discovered a new field, that I never understood or paid attention to before: “socks5 port”.

Well holy shit! \
Does this mean that my proxy is a socks5 proxy *too*, rather than only https?

I reconfigure the proxy in my browser to use socks5 instead of https, and provide the new port. \
I restart my browser and rush into youtube, to be completely dumbfounded by seeing that ***it works***.

Hoooooooly fucking shit I was paying for this proxy for more than half a year at this point, and all I fucking needed to do was realize that socks5 is a level up from https, rather than being some fancy other genre,
and that it's simply something I should be using everywhere I can,
to make more things work more easily.

# ok ok let's use this proxy, let's use this proxy in our apps

([/ref](https://www.youtube.com/watch?v=H1M7Fj6T91c))

Socks5 is the new nugget of information, because of which I wanted you to read more before acting; \
When buying a proxy, make sure it's *both* an https *and* a socks5 proxy —
that makes it much more powerful and appliable in more scenarios.

Let's start with the most common one!

## firefox

The firefox / floorp / zen / etc setup is pretty simple. \
It *should* be as simple in chromium-based browsers, for the record, but you'll have to find that out yourself :p

Install the [FoxyProxy extension](https://addons.mozilla.org/en-GB/firefox/addon/foxyproxy-standard/). \
Grant it the permissions it's asking for, and allow it to run in private windows. \
Go to the extension's settings → Proxies.

Here we're going to add our proxy *as a socks5 one*. \
Press the “Add” button, and fill in the hostname, port (specifically the socks5 one, not the https one!), username, password. \
Set “Type” to SOCKS5. \
I have “Proxy DNS” enabled, but I'm not sure if we need to honestly.

Now, add these patterns:
|cludation|pattern|
|-|-|
|include|\*|
|exclude|\*localhost\*|
|exclude|\*127.0.0.1\*|

As you use the proxy, you may find places that you specifically don't want to proxy, and will be able to exclude them here. \
Crucially, though, we begin by excluding localhost —
you don't want to proxy your own computer's local network.

You can also handle this the other way around: by **not** proxying anything by default, and adding stuff to proxy as you go. \
But if you're a russian reading this blog post, you probably want to proxy everything by default, due to how much stuff is blocked in russia nowadays. \
If you're in China, similar idea probably applies.

Now, press “Save” and enable the proxy by clicking on the extension button, and pressing “Proxy by patterns” —
That variant uses the patterns that we just provided above.

After a browser restart for good measure (shouldn't be needed usually), your proxy should be working! 🥳

## android

It is the socks5 revelation that let me use my proxy on my *phone* too, while in the past I could only use it helpfully on my pc. \
We're going to use an app called “Drony”. \
It's not on play store, so simply google “drony apk” and pick your favorite sketchy website to download it from.

In the app, swipe right to get to settings, go to “not wi-fi (mobile, ethernet, …)”. \
Fill in the hostname, port, username, password; set “Proxy type” to “SOCKS5”. \
Go back, and now press on “Wi-fi”. \
If you don't have any networks listed, you're probably chill already.
Otherwise, pick the first one probably, and fill in the details exactly like above, again.

Go back twice and swipe left back to the log tab, and press the “Off” button so that it's “On”. \
The proxy should now be working, globally, on your phone 🥳

After figuring this out, I have effectively doubled the value I get from my proxy for the price that I pay for it. \
2.67$ for my pc *and* phone? Insane!

I was really excited about how straightforward this solution is, and wanted to help out some russian that I know. \
Found a friend that was also doing the 5 vpns strat, and made them go through the setup process, using my proxy's information. \
It worked perfectly!

Kinda strange, isn't it? \
Usually, setting up something one *someone else*'s device is bound to not work in many strange ways, but this time everything just worked perfectly with no finagling. \
So it seems like it's reproducible! :D

“Works on my machine”? Nah! “Works on *your* machine”.

## etc

Now onto the more interesting to explain genre of proxiable things: “other”. \
I'm assuming you're on linux, btw.

There are these three interesting environment variables: `http_proxy`, `https_proxy`, `all_proxy`. \
One / some / all of them are understood by various apps, including but not limited to cli programs, to configure their proxy. \
Interestingly, they are indeed specified as lowercase, rather than uppercase, which is quite unusual for environment variables.

In my usage, I could never just stick to providing one of them, and for them to work, so the safest best is to provide all three. \
All of them use the `https://username:password@ip:port` syntax, so to launch some app with your proxy, you'd do something like:
```
http_proxy=https://username:password@ip:port https_proxy=https://username:password@ip:port all_proxy=https://username:password@ip:port some-app
```

I'm not 100% sure, but like 80% sure that you still can provide http*s* to the http_proxy variable, despite it seemingly expecting the proxy without TLS. \
Both `http_proxy` and `https_proxy` expect the *http* version of your proxy, rather than socks5; \
`all_proxy` doesn't have such a restriction, so you can provide the socks5 version of your proxy with `socks5://username:password@ip:port`.

But both of that, I believe, is a “usually” — `http_proxy` and `https_proxy` *might* allow socks5 in some apps, and sometimes `all_proxy` will not understand socks5.

All this “knowledge” is collected by me via experience and reading random stuff, so I'm very much farming corrections here :D \
Dm me on discord @Axlefublr or [email me](mailto:axlefublr.ls@gmail.com) if I'm wrong on some things directly above, I'd love to learn!

The 3 variables you can quite easily expect to work in various networking-related cli programs, like `curl`. \
But otherwise, their usefulness can be quite varied.

If a given program allows you to pass the proxy to it directly, via a flag or a config option or some such, *that* is much more likely to work. \
But the three ~~proxyteers~~ variables can potentially save your ass if you don't seem to have a more “proper” way to provide a proxy, yet they (the vars) surprisingly work.

Now let's talk about proxying something that *doesn't* care about the proxyteers.

## electron

Discord is an Electron app; \
I believe Electron doesn't come pre-built with `(https?|all)_proxy` support,
so whether you can use the variables depends on the specific app. \
Well, for discord it doesn't work at least, neither does it for vesktop.

However! I found a cool electron flag: `--proxy-server`. \
*This* should be supported in *any* electron app, as I assume electron takes care of the networking things, including the proxy. \
It has an interesting limitation that we haven't yet considered, though:

> Use a specified proxy server, which overrides the system setting. \
> This switch only affects requests with HTTP protocol, including HTTPS and WebSocket requests. \
> It is also noteworthy that not all proxy servers support HTTPS and WebSocket requests. \
> The proxy URL does not support username and password authentication per Chromium issue.

Our proxy requires a username and password, so we can't pass it directly into the flag, how we usually can. \
Electron not understanding username + password proxies might actually be the reason \
why `(https?|all)_proxy` don't work —
maybe they actually do, just not when username and password are required.

To work around this, we need to somehow convert our proxy to a non-passworded one, so that we could just provide the hostname:port.

### privoxy

↑ is the program that will allow us to do that!

Really frequently, various programs that allow you to do fancy things with proxies are really fucking complicated to make work. \
How often have you heard “just use wireguard”? And yet I find it extremely overwhelming... \
So I was both quite surprised and happy that privoxy *isn't* like that, and its setup was incredibly simple :D

First of all, install it of course.
On arch, it's just `privoxy` in the repos. \
On not arch, I hope it's also just as easily installable :3

The default config file is in `/etc/privoxy/config`. \
Add the following at the absolute end of it:
```
forward-socks5 / username:password@address:port .
```
With your *actual* username, password, ip address, and 🧦🧦🧦🧦🧦 port, of course.

That's the entire config!! It's literally that shrimple :>

You can now enable the systemd service that should come with your installation, with:
```sh
sudo systemctl enable --now privoxy
```
It will both start the privoxy service right *now*, **and** on subsequent logins. \
If you don't have the systemd service for one reason or another, \
just run it yourself with `privoxy /etc/privoxy/config` — it daemonizes by default anyway.

Right! I forgot to explain what it even does hehehehe

Basically, we're starting a uhhhhh thingy on our local network, into which we can send requests. \
I think it's not a lie to just call it a proxy? Yeah we're starting a proxy locally I think! \
So this local proxy can accept requests, and *relays* them to our main, *actual* (and passworded) proxy. \
Meaning that we get to specify an address + port without needing to specify the username + password 🥳

The “information” of privoxy is `127.0.0.1` for the ip address, and `8118` for the port, by default. \
You can check if it's actually working with this command:
```sh
curl --proxy http://127.0.0.1:8118 https://httpbin.org/ip
```

It should 1. not infinitely hold and 2. show you information about your *actual* proxy !

Coming back to the discord situation, I was eventually able to run it with:
```sh
discord --proxy-server=127.0.0.1:8118
```

Which is so awesome! \
I'm surprised I was able to figure out so much in one day and *actually* make discord work, rather than giving up. \
The power of proxies seems to scale indefinitely, directly proportional to your networking skills; \
Slowly but surely, I seem to gain those, due to the consequences of this stupid ass war. \
Although I sure wish I didn't have to do this out of requirement! lol

# final battle

Now using the discord *app*, I start to type out some message sharing my excitement; \
But I get stopped when trying to use yet another “fancy quote” or 🤯 or ←↓↑→ — the horror! \
Discord doesn't support `wtype`...

I have [a lot](@/erm/index.md) of direct hotkeys to various symbols and emojis, and *all* of them use `wtype` to be entered. \
This is a really big deal to me, much more than ctrl+number hotkeys :(

A pretty obvious first idea is “it's probably some wayland thing” — discord is probably running in xwayland mode unecessarily, so **w**type not working is not surprising. \
But funny thing is, discord *doesn't* have a wayland version anyway! How the fuck did this happen lmao!

So I try to handle this by using vesktop instead 🧐 \
There, forcing wayland *is* actually a thing you can do, but no dice on the wtype issue anyway :c

Since there is no one singular way to force wayland for apps, I decided to just keep throwing shit at the wall until it worked. \
I started with the basic variables and flags, but at the end I ended up with this monstrosity:
```sh
MOZ_ENABLE_WAYLAND=1 NATIVE_WAYLAND=1 ELECTRON_OZONE_PLATFORM_HINT=auto \
GDK_BACKEND=wayland QT_QPA_PLATFORM=wayland SDL_VIDEODRIVER=wayland \
vesktop --proxy-server=127.0.0.1:8118 --ozone-platform-hint=wayland \
--ozone-platform=wayland --enable-features=WaylandWindowDecorations \
--enable-features=WebRTCPipeWireCapturer --enable-features=UseOzonePlatform --gtk-version=4
```

The shit, to everyone's great surprise, did *not* work. How curious.

Actually, after I forced wayland (even the normal-er way), some hotkeys that *were* still somehow working, *stopped* working. \
By some wild magic, vesktop running in x11 mode supported *some* `wtype`s, but not most.
No clue what semantic it used, ngl. \
You'd think that **w**type would be better supported on **w**ayland, rather than x11, so that still throws me for a loop.

“Alright” I thought, \
“That's not a very fruitful thought” the reader replied, \
— I'll try webcord!

I'd much rather be using vesktop; a lot of people I know praise it to heaven, and I've heard of webcord like once or twice. \
But honestly I just want my symbol hotkeys to work in a discord that is also a separate app, so I'll take what I can get. \
Except, `wtype` doesn't work there either :/

I say `wtype`, but this isn't really program-specific! \
There's this global linux hotkey <kbd>ctrl+shift+u</kbd>, that allows you to type in a symbol's unicode, and replaces your input with the actual symbol. \
`wtype` uses the same mechanism for inserting the symbol, just skips the unicode input step.

This hotkey is really neat actually! \
I used to memorize unicodes and enter symbols directly using it, in the past 🤓 \
This was back on x11, where `xdotool type` was incredibly slow for me for whatever reason; so take note that the existence of the hotkey is not even wayland-specific!

The downside of it, as we now see, is that it doesn't work in *some* (incredibly rare) apps. \
Anki is one example, but I can easily forgo being able to use my symbol hotkeys there, as I use my own card adding solution anyway, that allows me to use helix instead. \
But a *chat* app?...

Conclusion: there is no way (yes, dear reader, I'm baiting you to potentially solve my entire problem if you happen to know how) to make wtype work on any discord apps that are worth using. \
This is a massive dealbreaker for me, soooo I guess I'm sticking with web discord?

You'd think I'm saying “aw dang” right about now, but actually I'm not that sad about how this adventure turned out! \
Sure, I end up without app discord, but in return I learned \
the difference between https and socks5 proxies, \
the fact that my proxy is *also* a socks5 one, \
how I can make use of socks5 to make the proxy more powerful,
***and*** work on mobile, \
helped a friend cave out of the same awful vpn situation, \
and learned how to strip my proxy off of its username + password, \
including now being able to proxy *any* electron applications, \
oh and I fixed spotify btw :D using the privoxy trick once again.

Similar to my [other journeys](@/my-usb-drive-experience.md), the reward of *knowledge* is much more valuable than the *practicality* of the result. \
Hell, I could conceivably sell my knowledge to normies, helping them set up a proxy! \
Is that a *job* I'm thinking about? 🤯

Considering the sheer price difference between a vpn, as well as (usually) much better performance, setting up a proxy for someone is a genuine service that will end up saving them money in the long term 🤔 something to think about :>

# footnotes

{{hn(i=1)}} I found this analogy hilarious more than anything, don't take it too seriously :D

{{hn(i=2)}} До сих пор злюсь немного на их ребрендинг. \
Какой нахуй Т-банк?? Тинькофф звучит намного более приятно.... \
Это как твиттер → х. \
Х блять!

{{hn(i=3)}} Благодаря чему эта, казалось бы рисковая, операция, на самом деле безопасная!

{{hn(i=4)}} These are all example values; the “information”s of your proxy will be different, but are going to *look* about the same, so this serves as a reference.

{{hn(i=5)}} Sheer guess.

{{hn(i=6)}} The wrong spelling is tribute to my friend skoove :3 \
It's one of her cutest misspellings

Am I overusing footnotes? \
I feel that you being able to jump *back* easily makes me spam them a lot more strongly than I should otherwise hahaha

{{hn(i=7)}} Reference to [Chuckle Sandwich](https://www.youtube.com/@ChuckleSandwich), while mentioning my puppygirl partner at the same time.
