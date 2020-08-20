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
> Plot a contour plot of the potential in the $$xy$$ plane for $$x \in [-a, a]$$ and $$y \in [-a, a]$$

The electric field is given by $$\vec{E} = -\nabla V_{\text{ext}} = -2V_0 \frac{z}{a^2}$$ and thus at the location of the Li atom 

$$\vec{E}(a/2 \hat{x} + a/2 \hat{y} + a \hat{z}) = -2V_0/a \hat{z} $$

we thus have 

$$\vec{p} = \alpha (-2V_0/a) \hat{z} $$

where $$\alpha/4 \pi \epsilon_0 = 24.3$$. The dipole has a potential

$$V_{\text{dip}}(\vec{r}) = \frac{1}{4 \pi \epsilon_0} \frac{\vec{p} \cdot \vec{\mathscr{r}}}{\mathscr{r}^3} = \frac{\alpha}{4 \pi \epsilon_0} \left(\frac{-2V_0}{a}\right) \frac{(z-a)^2}{[(x-a/2)^2+(y-a/2)^2+(z-a)^2]^{3/2}} $$

The total potential in the $$xy$$ plane is given by 

$$V_{\text{dip}}(x\hat{x}+y\hat{y}) + V_{\text{ext}}(x\hat{x}+y\hat{y}) =  \frac{\alpha}{4 \pi \epsilon_0} \left(\frac{-2V_0}{a^2}\right) \frac{1}{[(x/a-1/2)^2+(y/a-1/2)^2+1]^{3/2}}$$

First we define our potential as a function of $$x/a$$ and $$y/a$$


~~~
alph = 24.3

def potential(x_a, y_a):
    return -alph * 1 / ((x_a-0.5)**2+(y_a-0.5)**2+1)**(3/2)
~~~
{: .language-python}

We now use the meshgrid functionality to create 2D arrays of $$x$$, $$y$$, and the potential $$V(x,y)$$.

~~~
x = np.linspace(-2, 2, 100)
y = np.linspace(-2, 2, 100)
xv, yv = np.meshgrid(x, y)
pot = potential(xv, yv)
~~~
{: .language-python}

Since we are creating a color plot, we need to choose an appropriate color map. I like the spring style

~~~
from matplotlib import cm
cmap = cm.inferno
~~~
{: .language-python}

Now we plot. We choose a contour plot because it is interesting to examine lines of constant potential.

~~~
fig, ax = plt.subplots(figsize=(9,6))

cs = ax.contourf(xv, yv, pot, cmap=cmap, levels=12)
cbar = fig.colorbar(cs, ax=ax, extend='both', label=r'$\left(\frac{a^2}{2V_0}\right)|V|$')

ax.set_title('Magnitude of Potential in xy plane from Li Atom')
ax.set_xlabel('$x/a$')
ax.set_ylabel('$y/a$')

plt.show()
~~~
{: .language-python}

Lets go over the new code we used:

* `cs = ax.contourf(xv, yv, pot, cmap=cmap, levels=12)`. Here we are using the `contourf` function of matplotlib to create a filled contour plot. We input the meshgrid `xv` and `yv` and also the potential `pot`. We specify the colormap `cmap` and finally specify the number of levels used in the contour plot. The more levels, the more filled contours are drawn. Alternatively we can do a regular contour plot- that will be shown shortly.

* Note that above we used `cs= ax.contourf(...)` so we now have a "contour-plot object" `cs` defined as a variable. We will use this variable to generate a color bar. That is what we do in `cbar = fig.colorbar(cs, ax=ax, label=r'$\left(\frac{a^2}{2V_0}\right)|V|$')`. We specify the plot `cs` to generate the color bar for and the axes `ax` to draw the colorbar on. Finally we give it a LaTex label.

Alternatively, it might be better to create a standard contour plot where equipotential lines can be more readily obseved. We will need to adjust a few things in our code:

~~~
fig, ax = plt.subplots(figsize=(9,6))

cs = ax.contourf(xv, yv, pot, cmap=cmap, levels=12)
cbar = fig.colorbar(cs, ax=ax, extend='both', label=r'$\left(\frac{a^2}{2V_0}\right)|V|$')

ax.set_title('Magnitude of Potential in xy plane from Li Atom')
ax.set_xlabel('$x/a$')
ax.set_ylabel('$y/a$')

plt.show()
~~~
{: .language-python}






