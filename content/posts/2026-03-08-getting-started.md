---
title: Getting Started
description: "How and why I ended up writing Itinerant"
date: 2026-03-08T23:15:22.723Z
preview: ""
draft: false
tags: []
categories: []
---
It's difficult to know where to get started writing about Itinerant. It's something that's existed in some form or other for the past 10 - 15 years.

I first got started writing software in the 1980's when I was a kid. I was lucky in that my Dad always had computers in the house. One of my earliest memories is playing an instruments-only flight simulator on a ZX81. I must have been 5 or 6 years old. I started coding not long after. You kind of had to code to really do anything with a computer back then, and it fascinated me.

40 years later it still fascinates me.

I guess it was some time around 2012. By then I'd been writing software for a living for quite a while already - my first paying job happened out of the blue when I was about 16. Everything I'd written after then had been client-server web-based stuff or graphical-based software, in a wide variety of business fields. A lot of start-ups. Most frequently on my own. Then, a friend of mine got in touch asking me if I'd be interested in writing a procedural terrain generator for an open-world game he was a part of. They had money and wanted someone to be the CTO.

The game didn't really get going - the funding never came through. But I did write a procedural terrain generator that could work on Earth-sized planets, and it reignited something in me that brought me back to why I wanted to write software in the first place. When I was a kid, all I wanted to do was write games. It suddenly hit me how not only had I not played a game since I was a teenager, but I'd dropped writing games around that time as well and a lot of years had passed. Was I really doing the work I wanted to be doing? _(I mean, possibly yes, it does pay the bills, but still...)_

Suddenly I found myself wanting to write a game again and, even though that project failed, the idea of an open world game with planets, stars - a whole universe - stayed with me. I kept trying to put that idea aside and write simpler games that seemed more in-keeping with a solo developer working alone. A solo developer working alone can't build an immersive open-world game with Earth-sized planets, right?

But every time I tried to write something more.. achievable.. I got bored and went back to my procedural terrain stuff. I just loved working on it. But I never seemed to progress past some fancy graphics and a really vague idea of what I was doing.

Then around 5 or 6 years ago I hit on why I wasn't really getting anywhere with my ideas. I was getting stuck in a loop writing fancy procedural worlds that looked great, but then I'd run into roadblocks like, how do I make caves, or holes in the ground? How do I have multiple layers of geology? How do I save changes back to a server? What is the game actually about anyway? I was just making ray-traced clouds in the sky and neat shadows on land but that was it.

I think the main problem you run into as a solo developer working on something like this is that you really want to do the cool stuff - the big things - first. But the big things always seem to be the stuff you can show people - look at this cool planet I made! Look at how I used Rayleigh and Mie scattering to make the atmosphere look right! See how I have cliffs and grass and stuff! Look I made a procedural city! No, you can't go inside the buildings!

Well, fact is, people have already done all of that before. This isn't really one of the 'big thiings' you've got to solve when you're writing a game. It's a fun thing to work on for sure, but what you've actually done is taken a shortcut to show something that looks great, but now you've still got all the hard stuff to sort out. The stuff that bogs you down and makes it not fun to work on anymore. So you kinda slow down and stop, and then two or three years later you're dusting off your procedural world code because you want to see if you can make it look even better now and anyway, maybe you'll actually finish it this time. Except you don't, because all you actually do is succeed in making it look cooler, then get stuck on how to do something hard, you get bored again, life gets in the way and you move on until you get a new graphics card and wonder if you're old code will look even better again.

So it was COVID and I wanted to start writing my procedural open world game again and I just suddenly decided to scrap all the code I'd built up over the years and really ask myself what it was that I was trying to acheive.

At the time I thought I still wanted to write something with procedural worlds in it, but that it was important that you could 'edit' the planet in some way - by digging, destroying, building, whatever. It had to have that kind of vibe to it. I also wanted the planets to look as realistic as possible.. and then there had to be more variability in the planet that's more like a real planet, where everything's unique and it's not just like you're going from a small selection of predetermined environments with a noise function splattered over the top.

So I thought about it some more and I thought, really, I want something that's capable of making a planet that looks a bit like the Earth. Something that can have layered geology like the Earth. Something that looks less like the procedural noise-driven worlds we've seen over and over again and more like it could actually be like a real planet.

What I was telling myself is that I wanted to write a game that could potentially render the Earth itself. With procedural worlds people talk about how many 'biomes' they've got. Like, if you've got four biomes you're doing ok. Then you add a noise function that spreads the biomes out and presto! You've got a planet. But real planets don't work like that. They don't really have biomes as such but a variable set of parameters that generate different types of terrain, life and weather based on a complex set of factors working in unison.

For example, a desert on planet Earth can be somewhere really hot like the Gobi desert, or it can be somewhere really cold like Antarctica. Both are deserts but they look completely different and support different types of life. I figure there should be a way of predicting what appears at any point on a planet that doesn't involve looking it up on a list of biomes. Even if you have 20 or 30 biomes, once you've found all of them, it doesn't matter how many billions of procedural planets your game has, you've found everything that's worth finding and anything else is just going to be a repeat of the biomes you already discovered.

I realised that if I was to stand a chance of building realistic-looking procedural worlds that could look and work more like planet Earth, really the best place for me to start would be to build everything I needed to actually render planet Earth itself. Understanding this gave me a different challenge based around trying to come up with data structures that were able to describe surfaces (and underlying geology) that resembled real-life planets like the Earth.

That's how I got started on what has become Itinerant. It gave me a set focus that turned into a core philosophy that's helped me continually move forward with the game development and break out of the loop I'd been in:

1. Always do the difficult stuff first.
2. Don't get distracted by shiny things.
3. Let the game develop in its own time - there's no need to have all the answers right away.