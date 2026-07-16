---
tags:
  - note
  - super-musr
created: 2026-06-25
---
# Tags
[[MuSR]]

# Notes

- The muon has a magnetic moment 
- this is like a current loop 
- if a charged particle orbits, it makes a current loop, so we have a orbital magnetic moment, defined by current I multiplied by area A
- if a charged particle orbits, its mass forms angular momentum.

**The orbital magnetic moment $\mu$ is proportional to the angular momentum $L$ **

The equation that is of interest here is 

$$
\vec{\mu} = \gamma \vec{L}
$$
But what if the particle is not in orbit? 
$$
\vec{L} = 0
$$
The particles can still have intrinsic magnetic momentum connected to its intrinsic angular momentum. When this is the case the magnetic moment is given by: 
$$
\vec{\mu} = \gamma \vec{S}
$$
Where S is the spin angular momentum for a particle of mass m and charge q the gyromagnetic ratio is given by: 
$$
\gamma=\frac{gq}{2m}
$$
where g is  a constant known as the g-factor and for which the electron and muon is essentially 2. putting in the numbers gives us a interesting number 
$$
\frac{\gamma_{\mu}}{2\pi} = 135.5MHzT^{-1}
$$
## classical treatment 

magnetic moment $\mu$ in an applied field $\vec{B}$ has energy E given by 
$$
	E = -\vec{\mu} \cdot \vec{B} 
$$
Thus we might think that the magnetic field would cause the magnetic moment to line up with it, to minimise its energy. 

because the magnetic moment is associated with angular momentum, there is an torque $\mathbf{G}$ given by 
$$
 G = \mu \times B
$$
Note this is still in vector format
since toque is equal to the rate of change of angular momentum, we can rewrite the above equation as: 
$$
	\frac{d \mu }{dt} = \gamma \mu \times \mathbf{B}
$$
This means that change in $\mu$ is perpendicular to both $\mu$ and $\mathbf{B}$ the magnetic field causes the direction of $\mu$ to precess around $\mathbf{B}$ 
 
consider the case where b is along the z direction and $\mu$ is initially and angle of $\theta$ to $\mathbf{B}$ 
and in the xz-plane then: 
$$
	\dot{\mu}_{x} = \gamma B \mu_{y}
$$
$$
	\dot{\mu}_{y} = - \gamma B \mu_{x}
$$

$$
	\dot{\mu}_{z} = 0
$$

So that $\mu_{z}$ is constant with time and $\mu_{x}$ and $\mu_{y}$ both oscillate Solving the differential equations leads to:

$$
	\mu_{x}(t) =|\mu| \sin \theta \cos \omega t
$$
$$
	\mu_{y}(t) = - |\mu|\sin\theta \sin \omega t
$$
$$
	\mu_{z} = | \mu| \cos \theta
$$
where $\omega = \gamma B$ 
this is called the Larmor precession frequency 

## quantum mechanical treatment 
