---
layout: post
title:  "Preliminary Spritework, Level Completion"
excerpt_separator: <!--more-->
videoId: ggprn3Y7uYo
---

Happy Halloween! This week, I began to focus on some non-engineering tasks and started doing some spritework for the game. I additionally added some menu things (like the ability to traverse levels after completing them), and a hi-hat (with some design problems).

<!--more-->

## Spritework

I purchased [Aseprite](https://aseprite.org), the sprite editor/pixel art tool. I used it to begin doing some preliminary art for Project Legato.

Here's a screenshot of what the program looks like (with a sneak preview of one of the sprites):

![aseprite program](/assets/images/2019-10-30-aseprite.png)

## Level Completion

To record whether or not a player has completed a level, I created a simple object that they will hit if and only if they complete the level successfully. When the game detects level completion, it will enable a button that lets the player go to the next level. 

I decided to not pause the game upon level completion or force the player to the next level automatically, because ideally the player would like to review their solution, listen to it more times to understand how it works from a musical perspective, and then continue.

Additionally, I began implementing a pause button for players who will want to observe the entire level and place instruments before letting the level play out. Currently, it just modifies the game's timescale, so it doesn't support scrolling the camera while paused, but I plan on working on that in the future.

## Hi-Hat: Design Problems

![hi-hat sprite](/assets/images/2019-10-30-cymbal.png)

I ran into a bit of a brick wall this week when implementing the hi-hat cymbal. I added the sound in Wwise, implemented the `InstrumentController` class I mentioned last week, and got a visual representation going.

However, I was struck with a pretty hard problem (that I still am working on): For an instrument that's sometimes gonna be on the upbeat, and repeatedly hit in succession (see the progress video below at around 0:24), what type of game mechanic should this be?

I already have jumping and shooting. I thought about some sort of other movement mechanic, like the Mario "ground pound", but still couldn't come up with any good ideas. 

The best idea I've come up with so far is that on these upbeats spikes fall from the sky, and the hi-hat is a shield that you hold over your head. I'm not 100% sold on it, so I'm planning on getting feedback from my advisor and my peers to see what they think.

## Progress Video

{% include ytembed.html id=page.videoId %}

## Next Steps

* hi-hat game mechanic, more levels
* camera scroll on-screen buttons
* animated instruments (I want to animate them so they bounce when their instruments play!)