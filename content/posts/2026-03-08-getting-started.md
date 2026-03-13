---
title: Getting Started
description: "How and why I ended up writing Itinerant"
date: 2026-03-08T23:15:22.723Z
preview: ""
draft: false
tags: []
categories: []
---
It's difficult to know where to get started writing about Itinerant. It's existed in some form or other for the past 10 - 15 years.

I first started writing software in the 1980's when I was a kid. I was lucky in that my Dad always had computers in the house. One of my earliest memories is playing an instruments-only flight simulator on a ZX81. I must have been 5 or 6 years old. I started coding not long after. You kind of had to code to really do anything with a computer back then, and it fascinated me.

40 years later it still fascinates me.

I guess it was some time around 2012. By then I'd been writing software for a living for quite a while - my first paying job happened out of the blue when I was about 16. Everything I'd written after then had been client-server web-based stuff or graphical-based software, in a wide variety of business fields. A lot of start-ups. Most frequently on my own. Then, a friend of mine got in touch asking me if I'd be interested in writing a procedural terrain generator for an open-world game he was a part of. They had money and wanted someone to be the CTO.

The game didn't really get going - the funding never came through. But I did write a procedural terrain generator that could work on Earth-sized planets, and it reignited something in me. I had genuinely forgotten why I'd starting writing software in the first place. When I was a kid, all I wanted to do was write games. It suddenly hit me how not only had I not really played a game since I was a teenager, but I'd dropped writing games around that time as well and somehow a lot of years had passed. Was I really doing the work I wanted to be doing? _(I mean, possibly yes, it does pay the bills, but still...)_

Even though that project failed, I found myself wanting to write a game again, and the idea of an open world game with planets, stars - a whole universe - stayed with me. I kept trying to put that idea aside and write simpler games that seemed more in-keeping with a solo developer working alone. A solo developer working alone can't build an immersive open-world game with Earth-sized planets, right?

But every time I tried to write something more.. achievable.. I got found myself going back to my procedural terrain stuff. I just loved working on it. But I never seemed to progress past some fancy graphics and a really vague idea of what I was doing.

Then around 5 or 6 years ago I reflected on the fact that I kept returning to this procedural terrain stuff but never getting to the point of having an actual game. I think I hit on why it wasn't really working. I was getting stuck in a loop writing fancy procedural worlds that looked great, but then I'd run into roadblocks like, how do I make caves, or holes in the ground? How do I have multiple layers of geology? How do I save changes back to a server? What is the game actually about anyway? I was just making ray-traced clouds in the sky and neat shadows on land but that was it.

The fact is I was trying to run before I could walk. I had the knowledge and ability to code up terrain that looked kinda realistic, I could write shaders with nice looking atmospheric effects and things like that. I'd spend hours and hours very slowly improving that side of it, but really I was ignoring a lot of hard stuff that I didn't know how to do.

I now think one of the main problems you can run into as a solo developer working on something like this is that you really want to do the cool stuff - the big things - first. But the big things always seem to be the stuff you can show people - look at this cool planet I made! Look at how I used Rayleigh and Mie scattering to make the atmosphere look right! See how I have cliffs and grass and stuff! Look I made a procedural city! No, you can't go inside the buildings!

From there on, you can have foundational issues with the code, but because you're now familiar with shaders, terrain, the visual aspects, it can be very easy to just slip down a rabbit-hole of slowly and incrementally improving these same features and not progressing with potentially more important elements of the development.

The visual stuff _seems_ important, but it's not really. Most of it has been done before (how many 'individual blades of grass moving in the wind' demos have you seen?) - so you know it's acheivable. Really you should be spending time on the things that don't seem quite as acheivable.

So it was COVID and I wanted to start writing my procedural open world game again and I just decided to scrap all the code I'd built up over the years and really ask myself what it was that I was trying to acheive.

At the time I thought I still wanted to write something with procedural worlds in it, but that it was important that you could 'edit' the planet in some way - by digging, destroying, building, whatever. It had to have that kind of vibe to it. I also wanted the planets to look as realistic as possible.. and then there had to be more variability in the planet that's more like a real planet, where everything's unique and it's not just like you're going from a small selection of predetermined environments with a noise function splattered over the top.

So I thought about it some more and I thought, really, I want something that's capable of making a planet that looks a bit like the Earth. Something that can have layered geology like the Earth. Something that looks less like the procedural noise-driven worlds we've seen over and over again and more like it could actually be like a real planet.

What I was telling myself is that I wanted to write a game that could potentially render the Earth itself. With procedural worlds people talk about how many 'biomes' they've got. Like, if you've got four biomes you're doing ok. Then you add a noise function that spreads the biomes out and presto! You've got a planet. But real planets don't work like that. They don't really have biomes as such but a variable set of parameters that generate different types of terrain, life and weather based on a complex set of factors working in unison.

For example, a desert on planet Earth can be somewhere really hot like the Gobi desert, or it can be somewhere really cold like Antarctica. Both are deserts but they look completely different and support different types of life. I figure there should be a way of predicting what appears at any point on a planet that doesn't involve looking it up on a list of biomes. Even if you have 20 or 30 biomes, once you've found all of them, it doesn't matter how many billions of procedural planets your game has, you've found everything that's worth finding and anything else is just going to be a repeat of the biomes you already discovered.

I realised that if I was to stand a chance of building realistic-looking procedural worlds that could look and work more like planet Earth, really the best place for me to start would be to build everything I needed to actually render planet Earth itself. Understanding this gave me a different challenge based around trying to come up with data structures that were able to describe surfaces (and the underlying geology) that resembled real-life planets.

That's how I got started on what has become Itinerant. It gave me a set focus that turned into a core philosophy, and it's helped me continually move forward with the game development and break out of the loop I'd been in.

1. Always do the difficult stuff first.
2. Don't get distracted by shiny things.
3. Let the game develop in its own time - there's no need to have all the answers right away.