---
title: Orbital maneuvers
description: Changing orbital parameters using propulsion systems.
date: 2019-08-26 15:29
image:
  path: /assets/images/categories/OOSS.jpg
categories:
  - Space
tags:
- Orbits
- Orbital mechanics
- Orbital parameters
toc: true
toc_sticky: true
author_profile: false
---

From my [last post](https://t-neumann.github.io/space/OrbitalBasics/) you should have read up on the basics of orbits and orbital paremeters. Now while this is interesting by itself, changing orbits and moving to different orbits in order to dock to space stations, escape to different celestial bodies or de-orbit onto a bodies surface - this is the stuff that is now why we are actually doing this. So that is why this post moves more into orbital mechanics and some basic maneuvers for modifying orbits.

Orbital mechanics is a core discipline within space-mission design and control.
It focuses on spacecraft trajectories, including orbital maneuvers, orbital plane changes, and interplanetary transfers, and is used by mission planners to predict the results of propulsive maneuvers.

Now let's pretend we have some well-funded space agency, can do anything we want and do not have to fear killing our astronauts - if only there was some simulation to do this. This is were KSP comes into play.

## Vessel

We do not want to simply calculate orbits, we want some actual space ship with propulsion systems in the orbit so we can see the impact of our maneuvers live. For this purpose, I created already in endless hours a <i class="fab fa-github" aria-hidden="true"></i> <a href="https://github.com/t-neumann/ksp-garage">huge garage</a> of different more or less efficient vessels for exploring the KSP universe.

For this particular, I will be using my rather tiny [SSTO](https://en.wikipedia.org/wiki/Single-stage-to-orbit) *SlickOrbiter* consisting of 4 rapier engines which are hybrid engines with both air-breathing and liquid fuel modes. This I complement with an Atomic Rocket Motor engine for space maneuvers with far lower thrust but much higher efficiency ($$I_{SP}$$). I will definitely dedicate a couple of posts to propulsion systems, staging modes etc in a later time, for know just take it as it is.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/posts/Maneuvers/slickorbiter.gif" alt="Slick orbiter" width = "100%">
