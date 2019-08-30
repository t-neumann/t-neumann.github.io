---
title: Orbital basics
description: Basic definition, terminology and concepts of orbits
date: 2019-08-26 13:42
image:
  path: /assets/images/categories/OOSS.jpg
categories:
  - Space
tags:
- Orbital Mechanics
- Orbital Maneuvers
- Orbital Transfer
toc: true
toc_sticky: true
author_profile: false
gallery:
  - image_path: /assets/images/posts/Orbits/Newtonsmountainv=0.gif
    alt: "Newton's cannon v=0"
  - image_path: /assets/images/posts/Orbits/Newtonsmountainv=6000.gif
    alt: "Newton's cannon v=6000"
  - image_path: /assets/images/posts/Orbits/Newtonsmountainv=7300.gif
    alt: "Newton's cannon v=7300"
  - image_path: /assets/images/posts/Orbits/Newtonsmountainv=8000.gif
    alt: "Newton's cannon v=8000"
  - image_path: /assets/images/posts/Orbits/Newtonsmountainv=10000.gif
    alt: "Newton's cannon v=10000"
---

I was always fascinated by rockets, space in general and zero-gravity environments, however the Math's involved always deemed too complex for me. However, through the playful and still complex approach of [Kerbal Space Program](https://www.kerbalspaceprogram.com/) (KSP) - it is an awesome game I totally recommend to anybody remotely interested in space exploration - I picked up interest lately again and started reading into orbital mechanics, propulsion systems and related stuff in more detail.

This blog series is dedicated to summarising basic concepts at definitely super simplified and probably sometimes oversimplified and not entirely correct level.

The easiest concept for me to grasp, since once can do it quite interactively in KSP is the concept of orbits and orbital changes through orbital maneuvers.

So this very first post of this series will cover my basic understanding of the concept of orbits.

## Ellipse

Let's start of with refreshing our memory what an ellipse is - because that is what most relevant orbits for this blog series will look like. In mathematical terms, an ellipse is a plane curve surrounding two focal points ($$F_1$$ and $$F_2$$), such that for all points on the curve, the sum of the two distances $$d(F_1) + d(F_2)$$ is constant.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/posts/Orbits/Ellipse-definition.png" alt="Ellipse definition" width = "50%">

It is a generalization of a circle, where the two focal points are the same. Yes, also circular orbits exist.

### Ellipse parameters

There are a few important parameters describing an ellipse which will be referred throughout this blog series, so make sure you memorize and understand them, because they will keep popping up again and again.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/posts/Orbits/Ellipse-param.png" alt="Ellipse parameters" width = "50%">

###### Semi-major and semi-minor axes $$a \geq b$$

$$a$$ is referred to as the semi-major axis, i.e. $$a \geq b > 0$$.

###### Linear eccentricity $$c$$

This is the distance from the center to any of the two foci: $$c  = \sqrt{a^2 - b^2}$$.

###### Eccentricity $$e$$

The eccentricity is expressed as:

$$e = \frac{c}{a} = \sqrt{1 - (\frac{b}{a})^{2}}$$

assuming $$a > b$$. An ellipse with equal axes $$(a = b)$$ has zero eccentricity and is a circle.

###### Semi-latus rectum $$l$$

The length of the chord through one of the foci, perpendicular to the major axis, is called the latus rectum. One half of it is the semi-latus rectum $$l$$. A calculation shows:

$$l = \frac{b^2}{a} = a(1-e^2)$$


The semi-latus rectum $$l$$ is equal to the radius of curvature of the osculating circles at the vertices.

## Orbit

Now probably everybody has some idea what an orbit is, but before going into details, let's first summarise the definitions I found on the web.

#### Definition

In physics, an orbit is the gravitationally curved trajectory of an object, like the the trajectory of any plane around a star or a satellite around earth. Unless mentioned differently, in this blogpost orbit refers to a regularly repeating trajectory, but there are also non-repeating trajectories. To a close approximation, planets and satellites follow elliptic orbits, with the central mass being orbited at on of the two focal points of the ellipse, as described by [Kepler's laws of planetary motion](https://en.wikipedia.org/wiki/Kepler%27s_laws_of_planetary_motion).

The post will stick to the classical Newtonian mechanics paradigm of describing orbital motion, which is an adequate approximation for most situations. However, Einstein's generaly theory of relativity, which accounts for gravity as due to curvature of spacetime and orbits following geodesics, provides a more accurate calculation and understanding of the exact mechanics of orbital motion, which is needed in near very massive bodies (e.g. Mercury's orbit around the sun) or for extreme precision (as for GPS satellites).

#### Understanding orbits

There are two factors involved for understanding orbits:

* Gravity pulling an object from its straight path into a curved path
* The velocity at which this object is trying to travel along its path

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/posts/Orbits/tangentialvelocity.jpg" alt="Tangential velocity vs gravity" width = "50%">

This principal is illustrated by the illustration above, where gravity from a massive body in the center (green) pulls a object travelling on a straight path (pink object, black arrows), effectively bending the path with its constant pull (red) around the center body.

Another way how to illustrate how orbits develop is the though experiment of [Newton's cannonball](https://en.wikipedia.org/wiki/Newton%27s_cannonball). Here, we visualize a cannon on top of a very high mountain which can fire at any imaginable speed.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/posts/Orbits/Newton_Cannon.png" alt="Newton cannon" width = "50%">

If the cannon fires its ball with a low initial speed, the trajectory of the ball curves downward and hits the ground **(A)**. As the firing speed is increased, the cannonball hits the ground farther **(B)** away from the cannon, because while the ball is still falling towards the ground, the ground is increasingly curving away from it (see first point, above). All these motions are actually "orbits" in a technical sense – they are describing a portion of an elliptical path around the center of gravity – but the orbits are interrupted by striking the Earth. The horizontal speed for both **(A)** and **(B)** is 0 - 7,000 m/s for Earth.

If the cannonball is fired with sufficient speed, the ground curves away from the ball at least as much as the ball falls – so the ball never strikes the ground. It is now in what could be called a non-interrupted, or circumnavigating, orbit. For any specific combination of height above the center of gravity and mass of the planet, there is one specific firing speed (unaffected by the mass of the ball, which is assumed to be very small relative to the Earth's mass) that produces a circular orbit, as shown in **(C)**.

As the firing speed is increased beyond this, non-interrupted elliptic orbits are produced; one is shown in **(D)**. If the initial firing is above the surface of the Earth as shown, there will also be non-interrupted elliptical orbits at slower firing speed; these will come closest to the Earth at the point half an orbit beyond, and directly opposite the firing point, below the circular orbit. The horizontal speed for both **(C)** and **(D)** ranges from 7,300 to 10,000 m/s for Earth.

At a specific horizontal firing speed called escape velocity, dependent on the mass of the planet, an open orbit **(E)** is achieved that has a parabolic path. At even greater speeds the object will follow a range of hyperbolic trajectories. In a practical sense, both of these trajectory types mean the object is "breaking free" of the planet's gravity, and "going off into space" never to return. This involves any horizontal speed > 10,000 m/s for Earth.

{% include gallery caption="Various firing speeds of Newton's cannon and the resulting trajectory." %}

This leads to four practical classes of moving objects:

1. No orbit

1. Suborbital trajectories

  * Range of interupted elliptical paths

2. Orbital trajectories

  * Range of elliptical paths with closes point opposite firing point
  * Circular path
  * Range of elliptical paths with closes point at firing point

3. Open (escape) trajectories

  * Parabolic paths
  * Hyperbolic paths

#### Orbital elements

Orbital elements are the parameters required to uniquely identify a specific orbits. In celestial mechanices, usually a Kepler orbit is used. A real orbit changes over time due to gravitational perturbations by other objects and relativistic effects, so a Keplerian orbit is merely an idealized, mathematical approximation at a particular time.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/posts/Orbits/orbitalelements.png" alt="Orbital elements" width = "50%">

 An orbit is generally defined by six elements (known as Keplerian elements) that can be computed from position and velocity:

 Two define the size and shape of the trajectory (compare with [ellipse parameters](###Ellipse)):

 * Semimajor axis $$a$$
 * Eccentricity $$e$$

 Two elements define the orientation of the orbital plane in which the ellipse is embedded:

 * Inclination $$i$$ - vertical tilt of the ellipse with respect to the reference plane (for the earth e.g. the equatorial plane), measured at the ascending node. The ascending node is where the orbit passes upwards through the reference plane). The tilt angle is measured perpendicular to the line of intersection between the orbital plane and the reference plane.

 * Longitude of the ascending node $$\Omega$$ - horizontally orients the ascending node of the ellipse with respect to the reference frame's vernal point :aries:.
