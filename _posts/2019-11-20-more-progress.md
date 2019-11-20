---
layout: post
title:  "More and More Iteration"
excerpt_separator: <!--more-->
videoId: 4rRJFDM2goc
---

This week, I iterated based on feedback from a playtest session I had and continued to make progress to work towards the final design expo at the end of the semester

<!--more-->

## Playtest Feedback

I conducted a playtest of last week's version of the game with some friends unfamiliar with the game. It went mostly well, but there were two main pieces of feedback I got from them:

* *needs* a tutorial very badly
* hovering is unhelpful and actually very confusing


From the first piece of feedback, I worked with one of those friends to come up with what they felt was an intuitive, brief explanation of the game. With the second, I disabled/removed hovering completely from the game.

## Decoupling Wwise

I realized last week that if I plan on releasing on web (as a WebGL build), I can no longer be using Wwise for this project. Also, I wasn't really using any of the important, powerful features of Wwise that warranted it being used.

This mostly was seamless, the hardest part actually wasn't the actual decoupling of the two platforms, but to successfully remove all the Wwise-related assets and stuff that existed in the project, and to mix/balance the noise levels between the metronome and the instruments.

## Menu and Save Progress

I started working on a main menu and level select for the game. I realized that to make the level select effective I'd have to save the progress of the player as they completed levels to enable more and more level buttons.

I thought I might have to be saving JSONs/binaries and loading them to do this saving, but I realized I could just be using the Unity `PlayerPrefs` system instead. This made it very nice and (mostly) seamless to add in a save system.

When the player beats a level, I do this:

```cs
PlayerPrefs.SetInt("level", levelNum + 1);
```

This sets that the 'maximum level' that the player can play is the *next* level following the one they just beat.

Then, in the menu, where the buttons corresponding to each level populate the screen, each one runs this code:

```cs
GetComponent<Button>().interactable = PlayerPrefs.GetInt("level", 1) >= lvl;
```

And viola! Game saves work. (Hopefully.) (I'll have to test on whatever I build).


## Animations

As you see in the progress video below, I've added animations onto the instruments for when they get hit! This hopefully adds charm to the game and makes it more enjoyable.

## Real Title...?

I'm beginning to explore different titles for the game. The one in the video below, "Rhythm Reducer", is one of the names I'm thinking of. I also am considering "Gridbeat", "Beat Producer, Grid Reducer", and a few others.


## Level Creation

I collected a list of drum beats of progressively higher complexity that I'm going to use to make levels for next week. Stay tuned!

## Progress Video

Here's what the game looks like with the menu and tutorial!

{% include ytembed.html id=page.videoId %}


## Next Steps

* Level creation. Take what I have written in notes and put it in game!
* Explore building to WebGL/Android
* Work on poster for the design expo