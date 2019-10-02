---
layout: post
title:  "Prototyping: Translating Drums To Platforming"
excerpt_separator: <!--more-->
videoId: 18RTouMMh-misfS5Zyj91aE7gJ7eBt8N1
---

Prototyping work continued this week as I explored mapping placing instruments in the "DAW" like editor from last week to inputs given to a player on a platforming level.

<!--more-->


## Design Ideas

To begin, I used OneNote to draw out and design the ideas I had for the game. I had initially thought that the instruments the player places on the beats would map onto building a platforming level that an automatically moving character traverses, like this:

![original design concept](/assets/images/2019-10-02-original-idea.png)

However, I had trouble coming up with ideas for different platforming elements that the instruments would correspond to. How would this become a level-based puzzle platformer? Where would elements of the levels be placed? As I thought more and more about this design, I realized more and more that it might be worth exploring in another direction.

![player places enemies to kill](/assets/images/2019-10-02-killer-idea.png)

Another idea I thought about briefly (sketched above) was that the character has a fixed set of things it does (ducks on beat 1 and 3, as an example), and has a finite amount of health. The goal of the player would be to write a rhythm where the instruments corrseponded to enemies and obstacles which hurt the character when they did that fixed action on the beat (in this example, an enemy which attacks the character from underneath it would hurt the character as they are ducking to avoid things from above). If the character was successfully killed at the end of the 4 beats, then the player has succeeded at the level. This idea seemed interesting, but then another idea occurred to me.

I had gotten a bit too far into thinking about how the character was a "lemming" which was uncontrollable by the player, and the player would be manipulating the character's environment. Instead, what if the player's music controlled the character directly?

![player sets up controls](/assets/images/2019-10-02-iterated-idea.png)

In this example, the bass drum corresponds to the player jumping and the snare jump corresponds to the player shooting a gun. Placing the bass drum on 1 and 3 means the player jumps over the gaps they'd fall into in beats 1 and 3; shooting the laser on beat 2 would defeat the attacking enemy that exists on beat 2. I like this idea the best at the moment, so I decided to try and flesh out this idea in my Unity/Wwise prototype.



## Prototype Work

The first thing I had to set up was the ability for the user to switch between two "contexts" or modes - the "editor mode" where they were making music; and the "level mode" where they were watching their music influence the 2D platformer. I could not simply disable everything that needed to be in the editor view and enable everything that needed to be in the level view, as that would disable the metronome and instruments placed in the editor.

While setting up this context switching, I set up a rudimentary testing area for the platformer, and set up the player to be moving right synchronized to the duration of 4 beats on the metronome. What I came up with is shown below:


![player movement synchronized to metronome, resetting on beat 1](/assets/images/2019-10-02-player-mvmt-sync.gif)

The user is able to switch between the two, and when the player is in the level view, they still hear the metronome ticking the exact same speed as while in the editor view.

From here, I worked on setting up an event/messaging system between the level and editor view so that each instrument could map itself to a Wwise sound event AND a player action, both to occur on a specific beat. I tried to work on writing this system at a decent enough level that it would be somewhat salvageable in the future when I try to add more instruments.

With my system complete, I'm now able to place bass drum hits on the 4 beats and have them correspond to the player jumping on those beats in the level view! A video showing this is below.


{% include gdriveplayer.html id=page.videoId %}


## Next Steps

The next steps for this week are to refine the design of this rhythm game, cleanup the code to be somewhat more extensible, explore adding more instruments, and try to design 1-2 levels.

As a reminder, this is 1 of 3 planned "sub games" of Project Legato - 2 more are planned to be designed for teaching melody and harmony. I also hope to spend small amounts of time this week brainstorming for one of the other two games.