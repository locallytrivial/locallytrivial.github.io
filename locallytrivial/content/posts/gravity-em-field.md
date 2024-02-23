---
title: "Gravity of a Photon"
date: 2019-06-15
draft: false
categories:
- Physics
tags:
- Gravity
---

A friend, who focuses primarily on experimental particle physics, recently asked me an interesting question about gravity.
Specifically, he asked how the presence of electromagnetic fields impacts the gravitational field. Applying some modern-physics
reasoning, he proposed that electromagnetic fields should exert gravitational influence because photons have momentum that
can be viewed as mass in special relativity, and should interact gravitationally. I found this idea interesting, if a bit
interpretive, and answered with the precise formulation of the impact of the electromagnetic field on the curvature tensor.
He suggested I post this response in case anyone else finds it interesting; to colleagues working in classical or quantum gravity
this might appear a bit pedestrian.

The Einstein field equation (below) is arguably the most famous equation in gravity theory, second perhaps to Newton's inverse-square
law, and despite its seemingly-complex tensorial notation, expresses a beautifully elegant and simple concept. For those less
familiar, Einstein's general relativity reformulates gravitational interaction as a manifestation of the curvature of spacetime.
Specifically, Einstein's formulation makes concrete the relationship between the curvature of spacetime and the mass-energy content
 of spacetime [^1].

$$ R_{\mu\nu} - \frac{1}{2}Rg_{\mu\nu} + \Lambda g_{\mu\nu} = \frac{8\pi G}{c^4}T_{\mu\nu} \quad\quad\quad(1) $$

In the above equation (Einstein equation), the left-hand side contains terms related to curvature of spacetime, like the Ricci
tensor $R_{\mu\nu}$  and the metric $g_{\mu\nu}$. The right-hand side contains terms related to energy and matter content,
specifically $T_{\mu\nu}$. This latter term, called the _stress-energy_ tensor, is where the connection to the electromagnetic
field appears.

Various forms of matter contribute differently to the stress-energy tensor, and electromagnetic energy is no exception. The
equation below outlines the way in which the electromagnetic field contributes to the stress energy density of a particular
area of spacetime.

$$T^{\mu\nu}=\frac{1}{\mu_0}[F^{\mu\alpha}F^{\nu}{}_{\alpha}-\frac{1}{4}g^{\mu\nu} F\_{\alpha\beta}F^{\alpha\beta}]\quad\quad\quad(2)$$

Where $F_{\mu\nu}$ is the electromagnetic field tensor, also called the *Faraday* tensor, where $\mathbf{E} = E^i e_i$ and
$\mathbf{B} = B^i e_i$ are the familiar electric and magnetic vector fields. Note that in the below, I adopt the $c=1$
convention for convenience [^2].

$$F^{\mu\nu} = \begin{bmatrix}
	0 & -E^x & -E^y & -E^z \\\\
	E^x & 0 & -B^z & B^y \\\\
	E^y & B^z & 0 & -B^x \\\\
	E^z & -B^y & B^x & 0 \\\\
\end{bmatrix} \quad\quad\quad(3)$$

Thus, equation (2) tells us how the electromagnetic fields we are familiar with, namely $\mathbf{E}$ and $\mathbf{B}$,
contribute to the energy density of a region of spacetime. Then equation (1) builds on that result to relate the
electromagnetic fields to the curvature of spacetime, which as Einstein revealed, manifests as the gravitational field. So
yes, the conventional electromagnetic fields, and the photons that constitute them, impact the gravitational field despite
having no rest mass!

[^1]: C. W. Misner, K. S. Thorne, J. A. Wheeler, and D. Kaiser, Gravitation (Princeton University Press, Princeton, N.J, 2017).
[^2]: S. Carroll, Spacetime and Geometry: An Introduction to General Relativity, 3rd ed. (Pearson Education, 2013).
