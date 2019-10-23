---
layout: post
title:  "Metronome Redesign and Horizontal Scroll"
excerpt_separator: <!--more-->
videoId: HE7UvL8TjBs
---

This week, significant engineering progress was made in the form of horizontal scrolling in-level and re-engineering the metronome to utilize an arbitrary number of beats, and other small QoL improvements to the codebase to work towards a point of minimal engineering left and only content creation.

<!--more-->

## Metronome and Instrument Placement Redesign

I have completed my redesign of the entities and components that are used for the metronome and enabling/disabling instruments on beats. Previously, the architecture was something like this:

A `MetronomeController` owns 4 "beat" objects, each with its own collider. With a function repeating corresponding to the set bpm (beats per minute) of the scene, the function would call all the events which had subscribed to each beat, in order, and then repeat. If it was activating the events for beat 1, then it would play a different sound than the sounds for the other beat.s

Each instrument had an `InstrumentBeatPlacer` component; additionally, each instrument had **four colliders**, one for each beat in the scene. When one of these colliders was clicked, it would subscribe to the event corresponding to that beat that the `MetronomeController` owned, by giving it the function in the component who implements `InstrumentController`.

This was terrible. It worked, but it was absolutely rigid and unextensible.



After my rewrite, here's how the architecture is:

There are an arbitrary number of instruments in the scene. They only have colliders on their sprite of their image, and these colliders do not overlap in the `y` direction. Each instrument only implements `InstrumentController`.

A `BeatManager` is told about these instruments. told how many beats are in the scene, and for each beat, which metronome sound should be played for them (their `BeatType`). It automatically inserts into the scene a number of `Beat` objects, each with the correct `BeatType`. When the `BeatManager` detects a mouse click, it checks if the mouse click was in the y-range of any instrument, and x-range of any `Beat`. If it was, the `Manager` informs the `Beat` to notify that `InstrumentController` when it is that beat's time to sound.


What this means is that:

- For new instruments, I only have to implement `InstrumentController`, and then put the instrument in it's own y-range on screen, and tell the `BeatManager`.

```cs
public interface InstrumentController
{
    void MakeSound();
    void CharacterAction();
    string GetName();
}
```


- I can set an arbitrary number of beats, with any emphasis.

![beat manager](/assets/images/2019-10-23-beat-manager.png)

## Scrolling

Since levels can now be multiple screens long, I had to allow the player to scroll left and right. The two main approaches I could go with were setting literal limits on what the `x` value of the camera could be, or using colliders. I felt that the former might be easier given how the redesign for the metronome and beats went. It was easy; I simply set that value dynamically after the `BeatManager` generates all of the beats.


## Progress Video

Here's what the scroll + arbitrary number of beats looks like:

{% include ytembed.html id=page.videoId %}


## Next Steps

I need to:

- make the character's actions scale properly with the beats per minute of the screen; some of the values only work well at 120 bpm.
- attach scrolling to buttons on-screen, not just left/right arrow keys. This game should be playable with mouse only.
- add the third metronome beat type to Wwise.