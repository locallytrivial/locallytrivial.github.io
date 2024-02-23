---
title: "Fermi Problem: Galactic Spaghetti"
date: 2021-10-08
draft: false
categories:
- Physics
tags:
- Fermi Problem
series:
- Fermi Problem
---

Often in discussions about blackholes, the question arises of observer experience when falling into a blackhole. Due to
the extreme tidal forces experienced along such a path, the observer would be stretched out - a process colloquially
referred to as *spaghettification*. This week I would like to look at a happier use for spaghetti near blackholes.
If an observer in a stable circular orbit around the supermassive blackhole at the center of our galaxy were to use 
spaghetti as a tether, how much spaghetti would be required? Restated, the question of interest this week is:

> How much spaghetti would be required to form the smallest stable circle around the supermassive blackhole at the center of the Milky Way?

The term for such an orbit (of a massive test particle) is commonly called the *innermost stable circular orbit*, or ISCO
for short. For today's post, we'll pretend the "S" stands for *spaghetti*.


### ISCO Radius

To keep things simple, we take the case of a non-spinning blackhole, which is described by the Schwarzschild solution to
the Einstein equations. The radius of the spaghetti orbit in this case would be $3$ times the blackhole radius $R_S$,
which is also called the *Schwarzschild* radius. [^1]

$$R_{\mathrm{ISCO}} = 6 \frac{G M}{c^2} = 3 R_{S}$$

In the above, $G$ represents Newton's gravitational constant, $c$ represents the speed of light, and $M$ represents the
mass of the blackhole around which our spaghetti tether has been formed.

### Spaghetti Distance

Next, we'll need a relationship between the length of spaghetti $\ell$ and the mass of that spaghetti $m$. If we assume a fixed
cross-sectional area $\sigma$ then we can use the familiar relationship between the density $\rho$ and volume $V$ of matter:

$$m = \rho V = \rho \sigma \ell \implies \ell = \frac{m}{\rho \sigma}$$
We can estimate the cross-sectional area of the spaghetti strands $\sigma$ by using the radius of a spaghetti stick $r_s$
as $\sigma = \pi r_s^2$. Similarly, we can estimate the density of spaghetti in terms of a familiar spaghetti box!

$$\rho = \frac{m_{\mathrm{box}}}{V_{\mathrm{box}}} = \frac{m_{\mathrm{box}}}{\ell_{\mathrm{box}} h_{\mathrm{box}} w_{\mathrm{box}}}$$

Where $\ell_{\mathrm{box}}, h_{\mathrm{box}}, w_{\mathrm{box}}$ are the length, height, and width of a spaghetti box, respectively.


### Solution

Putting the above expressions together, we arrive at the innermost spaghetti circular orbit mass:

$$\begin{aligned}m &= \ell \rho \sigma = \left(2 \pi \frac{6 GM}{c^2}\right) \left( \frac{m_{\mathrm{box}}}{\ell_{\mathrm{box}} h_{\mathrm{box}} w_{\mathrm{box}}} \right) \left(\pi r_s^2\right)\\\\ &= \frac{12 \pi^2 r_s^2 G M m_{\mathrm{box}}}{c^2 \ell_{\mathrm{box}} h_{\mathrm{box}} w_{\mathrm{box}}}\\ \end{aligned}$$

Plugging in some numbers:

- Mass of a (galactic center) blackhole: $M \approx 4e6 M_{\odot} = 8 \times 10^{36}$ kg
- Radius of spaghetti strand: $r_s \approx 2 \times 10^{-3}$ m
- Newton's Gravitational constant $G \approx 7 \times 10^{-11} \mathrm{\ m^{3} kg^{-1} s^{-2}}$
- Mass of spaghetti box: $m_{\mathrm{box}} \approx 5 \times 10^{-1}$ kg
- Dimensions of spaghetti box: $(\ell_{\mathrm{box}}, h_{\mathrm{box}}, w_{\mathrm{box}}) \approx (25, 3, 7) \times 10^{-2}$ m
- Speed of light: $c \approx 3 \times 10^8\ \mathrm{ms^{-1}}$



The estimate is hidden below. Press the button below to see solution!

<button class="solution-button" onclick="getElementById('fermi-estimate').style.visibility='visible';"><b>Show
Estimate</b></button>

<p id="fermi-estimate" style="visibility: hidden;">2.8 * 10^9 kg</p>


What a large amount of pasta! To put this number in context, This amount of spaghetti could feed every human on Earth approximately $3$ times. 

[^1]: C. W. Misner, K. S. Thorne, J. A. Wheeler, and D. Kaiser, Gravitation (Princeton University Press, Princeton, N.J, 2017).
