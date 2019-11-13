---
layout: post
title:  "Iterating on the Puzzle Game"
excerpt_separator: <!--more-->
videoId: 9dhhonFz9W8
---

I spent this week doing some engineering, UI, and UX work to improve on the puzzle game idea. However, I had to struggle against a vile enemy... detecting hovering.

<!--more-->


## Engineering Work: Making Level Creation Nicer

One of the tasks I completed this week, I further abstracted the instrument into a single prefab that all instruments come from, and now I have to write zero code for a new instrument. This is all I have to do now for a new instrument:

* Make the sound, and create a Wwise event.
* Make the sprite for the instrment
* Put the Wwise event name and sprite into the prefab
* ???
* PROFIT


Next - building off what I started last week - I turned level writing into a Scriptable Object.

Now, I can write levels as a grid of numbers in this scriptable object, and I have to make sure the rest of the level is configured properly.

## Detecting Hovering... Why is this so hard???

So full disclosure - my code for this project is a lil hacky but it's robustly hacky, in that most of it is fairly extensible.

But never did I think I'd have so much problems with doing hovering.

## Current Click Detection Code

Currently (start of the week) after I detect a player clicking  I raycasting down from the mouse and see if it hit a clickable place, and then if the player has actually clicked, I apply the click.

Like this:

```cs
if (Input.GetMouseButtonDown(0))
{
    Vector3 mousePos = Camera.main.ScreenToWorldPoint(Input.mousePosition);
    Ray ray = Camera.main.ScreenPointToRay(Input.mousePosition);
    RaycastHit2D hit = Physics2D.GetRayIntersection(ray,Mathf.Infinity);

    if (hit.collider == null) return;

    for (int i = 0; i < beatObjects.Length; i++)
    {
        if (hit.collider == beatObjects[i].GetComponent<BoxCollider2D>())
        {
            float loc;
            Instrument newInstrument = GetInstrument(mousePos.y, out loc);
            if (newInstrument == null) return;
            beatObjects[i].GetComponent<Beat>().ToggleInstrument(newInstrument, loc);
        }
    }
}
```

My first thought was to be constantly raycasting, and checking for clicks within. If the player isn't clicking, I'll follow some sort of hover logic.

## Progress Video

{% include ytembed.html id=page.videoId %}


## Next Steps

* Content creation! Finally.
* More UI/UX polishing. This still feels clunky, and there are bugs in the hover code.