---
title: Orbital maneuvers
description: Changing orbital parameters using propulsion systems.
date: 2019-09-08 15:29
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
gallery:
  - image_path: /assets/images/posts/Maneuvers/orbitorientation.png
    alt: "Orbit orientation"
  - image_path: /assets/images/posts/Maneuvers/directions.png
    alt: "Directional markers"
---

From my [last post](https://t-neumann.github.io/space/OrbitalBasics/) you should have read up on the basics of orbits and orbital paremeters. Now while this is interesting by itself, changing orbits and moving to different orbits in order to dock to space stations, escape to different celestial bodies or de-orbit onto a bodies surface - this is the stuff that is now why we are actually doing this. So that is why this post moves more into orbital mechanics and some basic maneuvers for modifying orbits.

Orbital mechanics is a core discipline within space-mission design and control.
It focuses on spacecraft trajectories, including orbital maneuvers, orbital plane changes, and interplanetary transfers, and is used by mission planners to predict the results of propulsive maneuvers.

Now let's pretend we have some well-funded space agency, can do anything we want and do not have to fear killing our astronauts - if only there was some simulation to do this. This is were KSP comes into play.

## Vessel

We do not want to simply calculate orbits, we want some actual space ship with propulsion systems in the orbit so we can see the impact of our maneuvers live. For this purpose, I created already in endless hours a <i class="fab fa-github" aria-hidden="true"></i> <a href="https://github.com/t-neumann/ksp-garage">huge garage</a> of different more or less efficient vessels for exploring the KSP universe.

For this particular, I will be using my rather tiny [SSTO](https://en.wikipedia.org/wiki/Single-stage-to-orbit) *SlickOrbiter* consisting of 4 rapier engines which are hybrid engines with both air-breathing and liquid fuel modes. This I complement with an Atomic Rocket Motor engine for space maneuvers with far lower thrust but much higher efficiency ($$I_{SP}$$). I will definitely dedicate a couple of posts to propulsion systems, staging modes etc in a later time, for know just take it as it is.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/posts/Maneuvers/slickorbiter.gif" alt="Slick orbiter" width = "100%">

## Spacecraft orientation

Now before we perform and orbit maneuvers or burns, we need to agree on the different directions we can point our spacecraft and perform these burns. Naturally, since we are in 3-dimensional space, we have 3 axis along which we can orient ourselves, each axis having 2 directions.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/posts/Maneuvers/spacecraftorientation.png" alt="Spacecraft orientation" width = "100%">

#### Prograde and retrograde

These vectors run along the axis in which direction the spacecraft is moving along its orbit.

#### Normal and anti-normal

The normal vectors are perpendicular to the orbital plane.

#### Radial in and radial output

These vectors are parallel to the orbital plane, and perpendicular to the prograde vector. The radial (or radial-in) vector points inside the orbit, towards the focus of the orbit, while the anti-radial (or radial-out) vector points outside the orbit, away from the body.

## Orbital maneuvers

Ok now it is time to make a couple of burns into these directions and see how it affects our orbital parameters. To this end we set up maneuver nodes with directional indicators as shown below.

{% include gallery layout="single" caption="Orbital directions and directional markers." %}

#### Prograde and retrograde maneuvers

So we are at the apoapsis of our nearly circular orbit perfectly aligned with the equatorial plane (0 degrees inclination). Let's see what happens if we burn into prograde direction.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/posts/Maneuvers/progradeburn.gif" alt="Prograde burn" width = "50%">

As we can see, the apoapsis moves to the opposite end of our now elliptic orbit and we raised the orbit's altitude on the opposite side.

What if we do a retrograde burn?

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/posts/Maneuvers/retrogradeburn.gif" alt="Retrograde burn" width = "50%">

As we can see, the periapsis on the opposing side is lowered until we go suborbital, meaning the spacecraft will deorbit on its way to periapsis and either burn up in the atmosphere or crash on the planet (unless a proper landing procedure is initiated).

In summary, burning prograde will increase orbital velocity, raising the altitude of the orbit on the other side, while burning retrograde will decrease velocity and reduce the orbit altitude on the other side.

This is the most efficient way to change the orbital shape (specifically the most common case, raising or lowering apsides) so whenever possible these vectors should be used.

#### Normal and anti-normal maneuvers

Again we are at the apoapsis of our nearly circular orbit perfectly aligned with the equatorial plane (0 degrees inclination). Let's see what happens if we burn into normal direction.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/posts/Maneuvers/normalburn.gif" alt="Normal burn" width = "50%">

We see that the orbital inclination (the angle between the orbital and equatorial plane) changes.

These vectors are generally used to match the orbital inclination of another celestial body or craft, and the only time this is possible is when the current craft's orbit intersects the orbital plane of the target - at the ascending and descending nodes. We will get to this in a second.

#### Radial in and radial out maneuvers

One last time we are at the apoapsis of our nearly circular orbit perfectly aligned with the equatorial plane (0 degrees inclination). Let's see what happens if we burn into the radial out direction.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/posts/Maneuvers/radialoutburn.gif" alt="Radial out burn" width = "50%">

We see that the orbit start rotating around the craft like spinning a hula hoop with a stick. Radial burns are usually not an efficient way of adjusting one's path - it is generally more effective to use prograde and retrograde burns.
