---
layout: post
title:  "Prototyping: UI Redesign and More Instruments"
excerpt_separator: <!--more-->
videoId: TaN63lkIaSM
---

This week, I made design and engineering progress by working on reducing the two-tab view into a single window, and adding more instruments.

<!--more-->

## Design Work: It's Combine Time!

At the start of this week, the UI looks like this (this is a gif from last week's blogpost):

![the current UI with 2 tabs which player must switch between](/assets/images/2019-10-02-player-mvmt-sync.gif)

Some feedback I recieved from demonstrating my previous prototype was that it might be nicer to not have to context-switch between the "editor" view and the "level" view; the player would have a better time being able to edit the beat while seeing the character's activity change actively. I returned to my OneNote drawing board and this was the design that I came up with:

![the planned out consolidated interface](/assets/images/2019-10-08-consolidated-design.png)

After some fiddling around and re-engineering, here's the updated game view:

![the interface updated with the new design](/assets/images/2019-10-08-consolidated-impl.gif)


## Engineering: Add Snare Drum

Currently there's `one instrument`, the bass drum, in the game's prototype, and there's a _lot_ of hardcoding going on that makes this bass drum hook up to the heirarchy of instruments, metronome, and player. I worked on re-engineering how the code is structured to make this more productive.

The biggest change was to decouple functionality in the bass drum: There was the part of the code that tracks placing instrument hits on the beats, subscribing and unsubscribing to the events for the beats; and then there was the part of the code that handled making the bass drum sound and making the character jump. I chose to turn the latter into an interface, `InstrumentController`.

Once that decoupling was completed, I was able to very easily add a Snare Drum to the prototype. I just had to create bullets/shooting and enemy obstacles, and then chain my `SnareDrumController` (which implements the aforementioned interface) into the player's `Shoot` function.

{% include ytembed.html id=page.videoId %}

## Next Steps

I want to wrap up the work on the rhythm game prototyping this week - I will implement subdivision of the beats (cymbal on upbeat of eighth notes ftw!), and then the ability to "make the level end"/navigational stuff. If all goes well, I might get some design work going on the melody/harmony minigames.