---
title: The Spycrab. Why does it exist?
pubDate: April 21, 2024
description: Originally a Tumblr post
---

A Spycrab is an iconic animation bug for the Spy from Team Fortress 2. This bug was introduced since the game’s launch in 2007 and still exists today!

<iframe width="50%" height="315" class="center" src="https://www.youtube.com/embed/3dyQlZziHnw?si=Dws9KvJRA4JM5VmH" title="YouTube video player" frameborder="0"allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin"allowfullscreen></iframe>

This post will cover on what causes this bug happen, how to fix it, and why it SHOULDN’T be fixed.

### Debugging the Spycrab

First we need to know how do you become a Spycrab in TF2. To become a Spycrab the player have to pull out the disguise kit, crouch, look up, then start moving inany direction.

<img src="/blog/spycrab/spycrab_steps.png" class="center" width="80%" alt="">

This give us a clue on what’s going on. The issue is related to looking around while crouching with the Spy’s disguise kit. We can take a look at how Valve setupthe Spy’s animations since they provided the model sources in the Source SDK. Let’s take a look at spy_movement.qci since that’s where they handle all of theanimations related to moving around. From this point I will refer the disguise kit as PDA since that’s what it called in the sources. Looking at the entry for `Crouch_PDA` and `Crouch_Walk_PDA` everything seems to be written correctly. Theres nothing wrong with the code itself

`Line 15: $sequence Crouch_PDA PDA_crouch_idle loop alignto a_reference addlayer PDA_aimmatrix_crouch_idle  activity ACT_MP_CROUCH_PDA 1`

`Line 424: $MPCrouchWalkWithWeapon Crouch_Walk_PDA 0 0 0 0 0 0 0 0 PDA_crouch_walkN PDA_crouch_walkCenter PDA_aimmatrix_crouch_idle ACT_MP_CROUCHWALK_PDA$infoNode$`

This means that the issue is coming from one of the animations itself. Lets load up the PDA crouch animations in SFM and compare with the normal stand upanimations.

<video src="/blog/spycrab/pda_aimmatrix.webm" width="80%" autoplay loop class="center"></video>

Everything looks fine however there’s something odd with the `pda_aimmatrix_crouch_idle` animation. It’s in a different pose entirely! To summarize whata aim matrix do, the animator put the character in various poses mimicking that the character is looking around. The game will take those poses and blend between themdepending on what direction the player is looking. Generally you don’t want to stray way from the idle pose too much since it can cause potential problems whenblending between various poses at once.

With closer inspection, it seems the `pda_aimmatrix_crouch_idle` animation is actually an early version of the spy’s knife aim matrix animation. Here’sboth aim matrices side by side.

<video src="/blog/spycrab/knife_aimmatrix.webm" autoplay loop class="center" width="80%" ></video>

### The Fix

Now knowing that the bug is created by accidentally exporting another aim matrix animation for the wrong weapon. The fix is actually pretty simple! Withouttouching the animation files itself. We can go into the `spy_movement.qci` and replace any mention of `pda_aimmatrix_crouch_idle` with`pda_aimmatrix_idle` and that’s it! Compiling `spy_animations.qc` and loading up TF2 we will see the Spycrab no longer works.

<video src="/blog/spycrab/pda_fix.webm" autoplay loop muted class="center" width="80%" ></video>

But should this bug really be fixed?

This is the part where I tell you that the Spycrab bug should never be fixed in TF2. Even though this bug was stemmed from a mistake and it’s pretty simple to fix.This bug should never be fixed purely because it’s very important for the game’s history and community. After this bug was discovered and popularized back in 2008. Itspawns plenty of memes within the community and in-game references from community cosmetics and unusuals, an official rare taunt for the disguise kit, warpaints, anda poster from the map Carnival of Carnage a Halloween reskin of Doomsday.

<img src="/blog/spycrab/scroll_spycrab.png" width="80%" class="center" alt="">

Hopefully this post provide some interesting insight on how this iconic bug was created and the process of debugging animations in Source Engine!