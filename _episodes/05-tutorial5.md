---
title: "Tutorial 5"
teaching: 30
exercises: 30
questions:
- "Part 1: Creating contour plots in python"
objectives:
- "Create a contour plot of equipotential lines for a polarized atom"
keypoints:
- "matplotlib can be used to create contour plots; these plots are often important for showing equipotential lines"
---

> A Li atom is placed in a potential $$V_{\text{ext}}(\vec{r}) = V_0 \left(\frac{z}{a}\right)^2 $$ at a position $$\vec{r} =  (a/2, a/2, a)$$. 

# Part 1
> Plot a 2D color graph of the magntiude of the potential in the $$xy$$ plane for $$x \in [-a, a]$$ and $$y \in [-a, a]$$

The electric field is given by $$\vec{E} = -\nabla V_{\text{ext}} = -2V_0 \frac{z}{a^2}$$ and thus at the location of the Li atom 

$$\vec{E}(a/2 \hat{x} + a/2 \hat{y} + a \hat{z}) = -2V_0/a \hat{z} $$

we thus have 

$$\vec{p} = \alpha (-2V_0/a) \hat{z} $$

where $$\alpha/4 \pi \epsilon_0 = 24.3$$. The dipole has a potential

$$V_{\text{dip}}(\vec{r}) = \frac{1}{4 \pi \epsilon_0} \frac{\vec{p} \cdot \vec{\mathscr{r}}}{\mathscr{r}^3} = \frac{\alpha}{4 \pi \epsilon_0} \left(\frac{-2V_0}{a}\right) \frac{(z-a)^2}{[(x-a/2)^2+(y-a/2)^2+(z-a)^2]^{3/2}} $$

The total potential in the $$xy$$ plane is given by 

$$V_{\text{dip}}(x\hat{x}+y\hat{y}) + V_{\text{ext}}(x\hat{x}+y\hat{y}) =  \frac{\alpha}{4 \pi \epsilon_0} \left(-2V_0a\right) \frac{1}{[(x-a/2)^2+(y-a/2)^2+a^2]^{3/2}}$$

$$= \frac{\alpha}{4 \pi \epsilon_0} \left(\frac{-2V_0}{a^2}\right) \frac{1}{[(x/a-1/2)^2+(y/a-1/2)^2+1]^{3/2}} $$
