---
layout: post
title:  "Cutting Scope and Abstracting the Metronome"
excerpt_separator: <!--more-->
---

Updates for this week are fairly brief; here are some updates on some decisions that have been made about this project, and small amounts of engineering progress that were made.

<!--more-->

## Reducing the Scope of Project Legato

After much discussion and thought, I'm making the decision to descope Project Legato from 3 games (rhythm, melody, and harmony) and focus exclusively on the rhythm game which I've been working on so far. Here's some of the reasoning behind the decision.


Firstly, **my time this semester is finite** when it comes to what I have time to work on. I am unable to commit more than ~12 hours/week to Project Legato, and in many cases, I'm unfortunately only able to contribute less than that. As a result, progress has been slower than anticipated and I must work to become more focused and have more productive work time put into Project Legato.

Secondly, **quality control is very important** to me. I'm approaching working on Project Legato from the standpoint of wanting the final product to have a clean user experience. If I maintain the scope of Project Legato to have 3 major components which are bound to be fairly different from each other, I am creating very high requirements for myself to make all parts polished and working to the degree that I want them to work. This is not conducive to creating a proper scope for the time commitment I can manage.

Thirdly, I see **a lot of potential in the rhythm game**. I truly feel that by focusing the rest of the semester on this one experience that I can push it further than I could push all three games at once. I believe that the design I have in mind and have been prototyping can truly create an engaging, fun, and thoughtful educational game and I'd like to explore that for the rest of the semester.

## Milestones Moving Forward

With the new project scope in mind, here's the general list of things (in no particular order) that I'd like to accomplish in the coming weeks:

1. Generalize the metronome system
   * The metronome system should be abstracted such that I can either give it a simple meter and have it subdivide into eighth notes (1), or can give it a number of eighth notes per measure and specify which eighth notes are to be emphasized (2).
   * (2) seems like it'd be slightly simpler to implement, and seeing as I'll be the one using it for level design, I think I'll have an easier time using this (note that the second can be used to do the first - e.g. measure with 8 eighth notes, emphasis on the 0th, 2nd, 4th, and 6th for example.)
2. Scrolling
   * In order to let more complicated levels exist, I'll have to create a system to scroll left-right to view more eighth notes. This will scroll both the level and the game.
   * In order to have enough space for all the instruments, The instrument view should be vertically scrolling to let more instruments be seen?
     * this will need to be explored to see if that's the best approach. 
3. Learn more about drums
   * I'm gonna reach out to friends who are percussionists to get a better understanding of what they find to be important things that I should keep in mind for Project Legato.
4. Finalize/make concrete the set of instruments/gameplay mechanics
   * Currently, I have [bassdrum]/[jump] and [snare]/[shoot]. I'd like to properly determine which instruments should be included and design game mechanics and potential levels that utilize the game mechanics
   * The approach for this should be to first figure out the important drum beats (from part 2 above) and from the instruments required for that, do level design.


## Engineering Work: Metronome Redesign

I have begun to rework the metronome to have the abstracted things I need. I was unable to get lots of time in on the engineering this week, but I have generally determined more specific requirements of the system that will require other things to change.

1. Un-hardcode the 'beats'. The metronome controller will parameterize this and lay out the number of beats needed for the measure properly.
2. Un-hardcode the collisions for each instrument. Instead of separate beat colliders for each instrument, each beat will know what y-ranges correspond to what instruments, and act accordingly to toggle those instruments.

I have to design this with the scrolling (mentioned above) in mind.