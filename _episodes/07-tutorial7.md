---
title: "Tutorial 7"
teaching: 30
exercises: 30
questions:
- "Part 1: A challening integral problem"
objectives:
- "Use most of the skills developed in the tutorial so far to examine induced EMF"
keypoints:
- "I *love* dimensionless ratios..."
---

> Consider a loop of wire with radius $$R$$ located in the plane $$z=0$$ and centered at the origin. Consider also a sphere of constant magnetization $$\vec{M} = -M \hat{z}$$ with radius $$a$$ like the one considered in Griffith's Example 6.1. The sphere moves at constant velocity $$-v\hat{z}$$ and its center moves from $$(0, 0, 2R)$$ to $$(0, 0, -2R)$$. For this problem, it will be useful to write $$a = nR$$ where $$n$$ is some constant (less than 1 so the sphere can fit through the wire loop). The end goal of this problem is to plot the induced EMF in the wire loop as a function of time. To do this, however, we need to be very careful with our dimensionless variables.

Before doing anything, I recommend making a careful drawing of the problem, highlighting the current loop, the magnetized object, and the direction in which it moves. It is worth noting that

$$\varepsilon = -\frac{d\Phi}{dt} = -\frac{d}{dt} \oint \vec{B} \cdot d\vec{A} = \oint B_z dA $$

so the order of buisness will be finding the $$z$$ component of the magnetic field due to the magnetized sphere.


# Part 1
> Find $$B_z$$ in the plane $$z=0$$. When the volume of the magnetized object is not intersection the plane $$z=0$$, you can write this as one expression, but when the the magnetized object is intersecting the plane $$z=0$$, the magnetic field will need to be split into two seperate cases. Find all the times when the magnetized object is intersecting the plane $$z=0$$. For a given time $$t$$, how much area of the magnetized object is in the plane $$z=0$$?

The first thing to note is that there are **a lot** of undefined variables. In the end, we want everything in terms of dimensionless quantities (including the EMF); to do so, we will have to use many tricks.

From Griffith's Example 6.1, we know the the magnetic field of a magnetizied sphere at the origin is given be

$$\vec{B} = \begin{cases} \frac{\mu_0}{4\pi} \frac{1}{r^3} \left(3(\vec{m} \cdot \hat{r})\hat{r} - \vec{m} \right) & \text{inside sphere}\\
                          \frac{2}{3} \mu_0 \left(\frac{2}{3} \mu_0 \vec{M} \right) & \text{outside sphere}\end{cases}$$
                          
where $$\vec{m} = \frac{4}{3} \pi a^3 \vec{M}$$. For this problem, we will write everything in terms of $$m$$: the effective total dipole moment of the sphere.

Note that in this problem the sphere is not always at the origin and thus we need to replace $$\vec{r} \to \vec{r}-\vec{r}'$$. First lets examine what $$\vec{r}$$ and $$\vec{r}'$$ should be. Note that we will eventually be integrating the magnetic field over a circle of radius $$R$$ on the plane $$z=0$$; it thus makes sense to choose $$\vec{r} = (r\cos\phi, r\sin\phi, 0)$$. Meanwhile, the moving dipole gives off the same field as an ideal magnetic dipole at the center of the sphere. Thus we should choose $$\vec{r}' = (0, 0, h(t))$$ where $$h(t)$$ is the current height of the center of the sphere above the current loop. 

So what is $$h(t)$$? Since the dipole moves at constant velocity $$-v\hat{z}$$ and starts at $$z=2R$$ it should be obvious that $$h(t) = 2R-vt$$. We can now substitute everything in and get

$$B_z^{\text{outside sphere}} = \frac{\mu_0 m }{4 \pi}\left(\frac{-3h(t)^2}{(r^2+h(t)^2)^{5/2}} + \frac{1}{(r^2+h(t)^2)^{3/2}} \right)

writing this in terms of dimensionless quantities yields

$$\frac{4 \pi R^3}{\mu_0 m } B_z^{\text{outside sphere}} = \left(\frac{-3(h/R)^2}{((r/R)^2+(h/R)^2)^{5/2}} + \frac{1}{((r/R)^2+(h/R)^2)^{3/2}} \right)

meanwhile, with $$a = nR$$ we get 

$$\frac{4 \pi R^3}{\mu_0 m } B_z^{\text{inside sphere}} = \frac{-2}{n^3}



# Part 2
> Write a function that allows you to determine the magnetic flux through the current loop $$\Phi$$ at some time $$t$$ and for some proportionality constant $$n$$. You will need to be extra careful; remember that at certain times the magnetic sphere intersects the plane $$z=0$$ and thus the magnetic field is given by two different expressions depending on whether or not you are inside the magnet or not.

# Part 3
> Find the magnetic flux at 1000 time points from when the magnetic sphere goes from $$(0, 0, 2R)$$ to $$(0, 0, -2R)$$ for a value of $$n=0.1$$. Take the negative derivative to determine the EMF induced in the coil. Plot both the magnetic flux and the EMF as a function of time. Compare these plots to Figure 7.23 in Griffith's. Do the plots look different? Why?