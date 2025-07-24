+++
title = 'proxy things'
date = '2025-07-22'
draft = true
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
Не волнуйтесь, реальная коммиссия не буквально 30%, это больше для безопасности, чтобы потом в жопу не ебаться когда вдруг не хватает 0.003 биткоина.

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

{{hr(id="i-hate-the-russian-language")}}

Блять ненавижу писать по-русски, это намного сложнее чем по английски 😭 \
Но “бит” смешной так что надо было 😤

# по сусекам

You have an https proxy now! \
The proxy comes with a set of “information” that you're going to use to plug it into places. \
Let's name all of them, so that they're a bit more familiar!{{fn(i=4)}}

|thingy|namey|
|------|-----|
|162.3.164.178|ip address|
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

# ok ok let's use this proxy, let's use this proxy in our apps

([/ref](https://www.youtube.com/watch?v=H1M7Fj6T91c&pp=ygUaYmVhdCB0aGlzIGd1eSB3aXRoIGhhbW1lcnM%3D))

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

So at that point, I was paying for my own proxy to use for most things, and using my partner's socks4 proxy for youtube. \
Feels kinda bleghy, right? \
Shouldn't my own proxy already accomplish everything?

I realized my grave underunderstanding recently, while trying to set up discord. \
*Web* discord is just fine for me, but is crucially missing the ctrl+123456789 hotkeys, so I really wanted to try out vesktop to get those. \
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

Hoooooooly fucking shit I was paying for this proxy for more than half a year at this point, and all I fucking needed to do was realize that socks5 is a level up from https, rather than being some fancy other type,
and that it's simply something I should be using everywhere I can,
to make more things work more easily.

❗wait my proxy supports socks5??

❗firefox proxy config
❗ah wait, socks5 — and that's why I told you to read the blog first before buying the proxy, so you could check for socks5

❗hmmm I guess only http, so partner's workaround after floorp switch
❗try to discord, ah right needs proxy, wait how do I pass it, oh maybe socks5, oh how about this flag
❗hence privoxy

❗but whoops, I guess they don't support wtype

❗drony setup — wifi and not wifi, but otherwise very straightforward, make sure to use the socks5 thing
❗sharing the proxy with a friend now so the setup seems to be reproducible :D

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
