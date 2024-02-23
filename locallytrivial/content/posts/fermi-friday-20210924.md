---
title: "Fermi Problem: Baseball Blackhole"
date: 2021-09-24
draft: false
categories:
- Physics
tags:
- Fermi Problem
series:
- Fermi Problem
---

The question of interest this week is:

> If all baseballs ever used in an MLB game were compressed into a blackhole, how large would the blackhole be?

To estimate the blackhole radius $r_{BH}$, we'll need to compute the number of baseballs used in MLB games, translate that number to a total mass, then compute the blackhole radius of the corresponding mass.

### Counting Baseballs

We begin with a lower-bound on the number of baseballs used in all MLB games. If we define $n_u$ as the number of pitches that a ball is used for, then the problem translates to counting pitches. The pitches per game can be computed in terms of the number of pitches per inning $n_p$ and the number of innings per game $n_i$ as $n_p n_i$. The number of regular season games per year  can be given in terms of the number of games per team $n_{g}$, the number of teams $n_{t}$, and the number of postseason games $n_{ps}$ as $n_g n_t / 2 + n_{ps}$. Scaling by the number of seasons $n_s$, then the complete expression for total baseballs is then given by:

$$n_B = \frac{n_s n_p n_i \left(n_g n_t + 2 n_{ps}  \right)}{2 n_u}$$

### Blackhole Radius

To estimate the blackhole radius $r_{BH}$ of an arbitrary mass $m$, we use the well-known, elegant equation for the Schwarzschild event-horizon radius:

$$r_{s} = \frac{2 G m}{c^2}$$

### Solution

Putting the above expressions together, we arrive at the baseball black hole radius:

$$r_{b} = \frac{G m_B n_s n_p n_i \left(n_g n_t + 2 n_{ps}  \right)}{n_u c^2}$$

Plugging in some numbers:

- Mass of a baseball: $m_B = 1.5 \times 10^{-1}$ kg
- Number of pitches per baseball: $n_u = 2 \times 10^0$
- Number of seasons: $n_s = 1.0 \times 10^2$
- Number of pitches per inning: $n_p = 2 \times 10^1$
- Number of innings per game: $n_i = 1 \times 10^1$
- Number of games per team: $n_g = 1.6 \times 10^2$
- Number of teams: $n_t = 3 \times 10^1$
- Number of post-season games: $n_{ps} = 5 \times 10^1$
- Gravitational constant: $G = 6.7 \times 10^{-11}\ \text{m}^3 \text{kg}^{-1}\text{s}^{-2}$
- Speed of Light: $c = 3 \times 10^8\ \text{ms}^{-1}$


<br>
The estimate is hidden below. Press the button below to see solution!

<button class="solution-button" onclick="getElementById('fermi-estimate').style.visibility='visible';"><b>Show
Estimate</b></button>

<p id="fermi-estimate" style="visibility: hidden;">5.4 * 10^-21 m</p>


The radius is so small, that I had to compute a few other numbers to contextualize it. The following numbers are the amount of time required, at the current rate of game play, for the baseball black hole to reach a certain size.

- Proton Radius ($10^{-15}$ m) would require 18 million more years of baseball
- Baseball Radius ($10^{-2}$ m) would require $1.3 \times 10^{21}$ more years of baseball. Considerably longer than the age of the universe ($1.4 \times 10^{10}$ yr)
    
