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
---

I was always fascinated by rockets, space in general and zero-gravity environments, however the Math's involved always deemed too complex for me. However, through the playful and still complex approach of [Kerbal Space Program](https://www.kerbalspaceprogram.com/) (KSP) - it is an awesome game I totally recommend to anybody remotely interested in space exploration - I picked up interest lately again and started reading into orbital mechanics, propulsion systems and related stuff in more detail.

This blog series is dedicated to summarising basic concepts at definitely super simplified and probably sometimes oversimplified and not entirely correct level.

The easiest concept for me to grasp, since once can do it quite interactively in KSP is the concept of orbits and orbital changes through orbital maneuvers.

So this very first post of this series will cover my basic understanding of the concept of orbits.

## Ellipse

Let's start of with refreshing our memory what an ellipse is - because that is what most relevant orbits for this blog series will look like. In mathematical terms, an ellipse is a plane curve surrounding two focal points (F1 and F2), such that for all points on the curve, the sum of the two distances dF1 + dF2 is constant.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/posts/Orbits/Ellipse-definition.png" alt="Ellipse definition">

It is a generalization of a circle, where the two focal points are the same. Yes, also circular orbits exist.

### Ellipse parameters

There are a few important parameters describing an ellipse which will be referred throughout this blog series, so make sure you memorize and understand them, because they will keep popping up again and again.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/posts/Orbits/Ellipse-param.png" alt="Ellipse parameters">

###### Semi-major and semi-minor axes $$a \geq b$$

$$a$$ is referred to as the semi-major axis, i.e. $$a \geq b > 0$$.

###### Linear eccentricity $$c$$

This is the distance from the center to any of the two foci: $$c  = \sqrt{a^2 - b^2}$$.




$$mean = \frac{\displaystyle\sum_{i=1}^{n} x_{i}}{n}$$
