---
layout: post
title:  "Prototyping: Drum Beats with a Metronome"
excerpt_separator: <!--more-->
---

This week, I began prototyping some of the elements for the rhythm game. I installed [AudioKinetic Wwise](https://www.audiokinetic.com/products/wwise/), the interactive audio engine, along with its Unity plugin. I tried to prototype a system wherein one could toggle on and off various drum sounds on 4 different beats of a metronome.

<!--more-->

## Installing Wwise

Downloading and installing [AudioKinetic Wwise](https://www.audiokinetic.com/products/wwise/) was for the most part straightforward; I ran into some issues verifying my email for creating and AudioKinetic account. The process that to them should've been instantaneous had me waiting for ~30 minutes for an email that let my verify my account. From there, I was allowed to download the Launcher, which, similar to Unity Hub, allows the user to install various versions of Wwise. You can additionally install plugins (for various features like spatial audio) as well as the integrations with the Unity and Unreal engines.


## Unity, Part 1


And I worked on creating a system for placing drum hits on a repeating metronome beat in Unity.

I started off by creating a metronome:

![metronome in unity](/img/2019-09-21-metronome.gif)

This metronome runs at 120 bpm (2 beats per second). There's no audio here so far; I simply made sure the metronome could handle the difference between the down beat (which gets highlighted green) and the other beats (which get highlighted black).


## Using Wwise

I downloaded some sounds from [freesound](https://freesound.org):

- Metronome tones [(link)](https://freesound.org/people/Druminfected/packs/15379/)
- Kick Drum hit [(link)](https://freesound.org/people/karolist/sounds/371192/)

To connect Wwise to my Unity project, it was actually as simple as going to the "Unity" tab in Wwise and selecting "Integrate into Unity Project".

![integrating wwise into unity project](/img/2019-09-21-integrating-wwise.png)

Then, I could relaunch Unity and launch Wwise and get to work!

In Wwise, I added the sounds I downloaded and was able to use Wwise's mixer to balance the sound (I want the bass drum hit to be louder than the metronome).

Then, I created events in Wwise that related to playing the three different sounds: the metronome on either beat 1, or beat 2/3/4; and the bass drum.

![wwise events](/img/2019-09-21-wwise-events.png)

## Unity, Part 2

Once I made the events, I went into Unity and found the Wwise plugin had been successfully integrated into Unity.

I saw that a `WwiseGlobal` object had been added to the scene:

![WwiseGlobal](/img/2019-09-21-inspector.png)

I thought I'd be all set to go ahead! I went into the C# script where I process the downbeat of the metronome and added the following line:

```cs
AkSoundEngine.PostEvent("DownBeat", this.gameObject);
```

And lo and behold, it didn't work!

This was alarming as I thought I had been following the (notably well written) [Wwise docs](https://www.audiokinetic.com/library/edge/?source=Help&id=wwise_help) pretty well. Turns out I missed a few steps:

Firstly, I had to use the Wwise Unity plugin to generate a package of sounds and associated events called a "SoundBank" into my project; and then I had to add the SoundBank as a component to an object in the scene (I added it to the `WwiseGlobal` object).

![Generating SoundBank](/img/2019-09-21-soundbank.png)

Finally, I had to ensure that following the posting of the event, I called the `AkSoundEngine::RenderAudio` function.

```cs
if (currIndex == 0)
{
    AkSoundEngine.PostEvent("DownBeat", this.gameObject);
    beats[currIndex].color = Color.green;
} else
{
    AkSoundEngine.PostEvent("Beat", this.gameObject);
    beats[currIndex].color = Color.red;
}
AkSoundEngine.RenderAudio();
```

And it worked! Running the project had the metronome correctly blipping on the beat.


Next, I added a bass drum object with colliders on every beat, that subscribed to a C# event activated in the metronome.

The bass drum object looked like this in the scene:

![bass drum in unity](/img/2019-09-21-bassdrumadded.png)

This was my callback in the bass drum:

```cs
public void HitDrum()
{
    AkSoundEngine.PostEvent("KickHit", this.gameObject);
}
```

and I just had to call the callback before the `RenderAudio()` call:

```cs
if (currIndex == 0)
{
    AkSoundEngine.PostEvent("DownBeat", this.gameObject);
    beats[currIndex].color = Color.green;
} else
{
    AkSoundEngine.PostEvent("Beat", this.gameObject);
    beats[currIndex].color = Color.red;
}
instrumentsForBeats[currIndex]();
AkSoundEngine.RenderAudio();
```

This gave me a working prototype where I can toggle the bass drum from hitting on the four beats of the measure as it loops. Awesome!