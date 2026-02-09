---
title: "Fermi Problem: A Different Manhattan Project"
date: 2021-09-17
draft: false
categories:
- Physics
tags:
- Fermi Problem
series:
- Fermi Problem
---

For this week's installment of Fermi estimation, the question of interest is:

> How fast would the sun burn an amount of hydrogen equivalent to the above-ground mass of Manhattan?

To estimate the burn-time we'll need to guess at a few quantities: the above-ground mass of Manhattan, the solar
luminosity (total energy emitted from the sun), and fraction of energy emitted from the core of the sun that makes it 
to the surface.

### Mass of Manhattan

We will estimate the above-ground mass $m$ of Manhattan by computing a rough volume $V$ and density $\rho$, then using the 
familiar relation $m=\rho V$. To compute the volume we make some assumptions. First, we represent the average number of 
stories of a building in Manhattan as $n_{s}$, the height of a story as $h_{s}$, and the number of buildings in Manhattan
as $n_{b}$. For simplicity, we assume each building to have a square base of length $b_{s}$, then the total volume of 
buildings in Manhattan $V_{\text{M}}$ is:

$$V_{\text{M}} = n_{b} n_{s} h_{s} b_{s}^2$$

Computing the density will be a little easier. If we assume that each building is roughly a shell of concrete, then concrete
will compose a fraction $f_{c}$ of the building. Given the density of concrete $\rho_{c}$, the density of Manhattan $$ 
will be $\rho_{\text{M}} = f_c \rho_{c}$. Plugging this in to get the above-ground mass of Manhattan:

$$m_{\text{M}} = V_{\text{M}} \rho_{\text{M}} = n_{b} n_{s} h_{s} b_{s}^2 f_c \rho_{c}$$


### Solar Luminosity

Using the temperature $T_{\odot}$ and radius $R_{\odot}$ of the sun, and approximating the Sun as a blackbordy, we can 
use the Stefan-Boltzman law. This law relates the temperature $T$ and surface area $A$ of a blackbody to the total 
energy output $\mathcal{L}$ of the object: $\mathcal{L} = \sigma A T^4$. For simplicity, we assume the sun to be a perfect
sphere, then:

$$\mathcal{L}_{\odot} = 4 \pi \sigma R_{\odot}^2 T_{\odot}^4$$


### Burn Rate

THe solar luminosity tells us how much energy per unit time is emitted from the sun. If we want to know how much
mass is consumed to produce that energy, we need to understand how much energy is produced in the fusion of Hydrogen.
Using a simple approximation, 4 Hydrogen atoms are converted into 1 Helium atom. The difference in total mass is due
to the energy released by the process, which we can estimate using Einstein's famous relation: $E=mc^2$. Therefore, 
the reaction energy $E_{r}$ is a function of the change in reactant mass $\Delta m_r = 4 m_{\text{H}} - m_{\text{He}}$, 
which gives us an expression for the unit energy $E_{\text{H}}$.  

$$E_{\text{H}} = \frac{E_r}{m_{\text{H}}} = \left(4 - \frac{m_{\text{He}}}{m_{\text{H}}} \right) c^2$$

We can now compute the burn rate by unit analysis. If the luminosity gives (energy / time) and the 
unit reaction energy gives (energy / mass), then in order to get the burn rate in terms of (mass / time) we need
to divide the luminosity by the unit reaction rate:

$$r_{\text{burn}} = \frac{\mathcal{L}}{E_{\text{H}}}$$


### Solution

Putting the above expressions together, we arrive at the burn-time:

$$\tau_{\text{burn}} = \frac{m_{\text{M}}}{r_{\text{burn}}} = \frac{m_{\text{M}} E_{\text{H}}  }{\mathcal{L}  }
= \frac{ n_{b} n_{s} h_{s} b_{s}^2 f_c \rho_{c} \left(4 - \frac{m_{\text{He}}}{m_{\text{H}}} \right) c^2 }{ 4 \pi \sigma R_{\odot}^2 T_{\odot}^4 }$$

Plugging in some numbers:

- Number of buildings in Manhattan: $n_b = 5 \times 10^4$
- Average number of stories: $n_s = 2 \times 10^1$
- Height of a story: $h_s = 5 \times 10^0$ (m)
- Width of a story: $b_s = 5 \times 10^1$ (m)
- Fraction of a building that is concrete: $f_c = 1 \times 10^{-3}$
- Density of concrete: $\rho_c = 3 \times 10^3$ (kg / m^3)
- Mass of Helium: $m_{\text{He}} = 6.6 \times 10^{-27}$ (kg)
- Mass of Hydrogen: $m_{\text{H}} = 1.7 \times 10^{-27}$ (kg)
- Speed of Light: $c = 3 \times 10^8$ (m / s)
- Temperature of sun: $T_{\odot} = 6 \times 10^3$ K (Kelvin)
- Radius of sun: $R_{\odot} = 7 \times 10^8$ m (meters)
- Stefan-Boltzmann constant: $\sigma = 6 \times 10^-8 \quad \text{W} \cdot \text{m}^{-2} \cdot \text{K}^{-4}$ 


<br>
The estimate is hidden below. Press the button below to see solution!

<button class="solution-button" onclick="getElementById('fermi-estimate').style.visibility='visible';"><b>Show
Estimate</b></button>

<p id="fermi-estimate" style="visibility: hidden;">8.7 * 10^-1 s</p>
    


