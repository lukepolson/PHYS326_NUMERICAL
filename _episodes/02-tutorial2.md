---
title: "Tutorial 2020/09/30"
teaching: 30
exercises: 30
questions:
- "Part 1: Evaluating numerical integrals to obtain potential"
- "Part 2: Fitting data to curves"
objectives:
- "Practice evaluating numerical integrals in python"
- "Learn to use the `curve_fit` function in scipy"
keypoints:
- "One can use numerical integration to solve for the potential of complicated charge distributions"
- "SciPy has functions for fitting data to complicated funcitonal forms"
---

# Preliminary
These are the packages we will use:

~~~
import numpy as np
import matplotlib.pyplot as plt
~~~
{: .language-python}

Consider a uniform line-charge distribution parametetrized by $$\theta$$ with total charge $$Q$$ located at

$$\vec{r} = (R\cos(\theta), R\sin(\theta), R\theta)$$

for $$-2\pi \leq \theta \leq 2\pi$$. This represents a ''spring'' of charge of with total $$Q$$. 


# Part 1
> Plot the potential along the $$x$$ axis for $$x:2R \to 10R$$.

It can be shown that

$$V(\vec{r}) = \frac{Q}{16 \pi^2 \epsilon_0} \int_{-2\pi}^{2 \pi} \frac{1}{\sqrt{(x-R\cos(\theta'))^2+(y-R\sin(\theta'))^2 + (z-R\theta')^2}} d\theta' $$

However, for plotting, we need to plot a dimensionless quantity. We can rearange the integral to be dimensionless and obtain the following expression:

$$\left( \frac{4 \pi \epsilon_0 R}{Q} \right)V(\vec{r}) = \frac{1}{4\pi}\int_{-2\pi}^{2\pi}\frac{1}{\sqrt{(x/R-\cos(\theta'))^2+(y/R-\sin(\theta'))^2+(z/R-\theta')^2}}d\theta'$$

Note that I keep a factor of $$4 \pi \epsilon_0$$ together. Now we define the integrand and the "potential" (by "potential" I really mean the dimensionless $$\left( \frac{8 \pi^2 \epsilon_0 R}{Q} \right)V(\vec{r})$$).

~~~
def integrand(theta, x_R, y_R, z_R):
    return 1/np.sqrt((x_R-np.cos(theta))**2+(y_R-np.sin(theta))**2+(z_R-theta)**2)
def potential(x_R, y_R, z_R):
    return 1/(4*np.pi) * quad(integrand, -2*np.pi, 2*np.pi, args=(x_R, y_R, z_R))[0]
~~~
{: .language-python}

Now we evaluate the potential for many different values of $$x$$ from $$x/R: 2 \to 10$$.

~~~
x_R = np.linspace(2,10,1000)
potentials = np.vectorize(potential)(x_R, 0, 0)
~~~
{: .language-python}

And now we can plot like we've done in tutorial 1.

~~~
plt.plot(x_R, potentials)
plt.xlabel('$x/R$', fontsize=14)
plt.ylabel(r'$\left( \frac{4 \pi \epsilon_0 R}{Q} \right)V(x\hat{x})$', fontsize=14)
plt.grid()
plt.title('Potential from Charged Spring')
plt.show()
~~~
{: .language-python}

# Part 2
> For the region $$x:100R \to 110R$$, fit the potential to the curve $$f(x;a)=\frac{a}{x/R}$$. What value of $$a$$ do you get? Then, using this value of $$a$$, plot $$V(x\hat{x})$$ and $$f(x;a)$$ on the same plot in 
> * the region $$x:2R \to 5R$$.
> * the region $$x:200R \to 210R$$

You will recall fitting a best fit line to data points in some of your first and second year labs. Generally when fitting a curve you need
1. Some data which I will call `xdata` and `ydata`
2. Some function that you believe accurately represents the data which I will call `f`.

We need to ask ourselves: what is `xdata` and `ydata` and what is the function `f`? In this case we were told $$f(x;a) = \frac{a}{x/R}$$. I include a ";" rather than a "," in $$f(x;a)$$ because $$x$$ is the domain of the function but $$a$$ is some parameter that we adjust. Why did we choose this functional form? Well we know that $$V(r) \sim 1/r$$ for small point charges. As we move farther and farther from the spring, it will look more and more like a point charge and thus the potential should have this form. Lets define this function

~~~
def f(x_R,a):
    return a/x_R
~~~
{: .language-python}



In this case our `xdata` is some points in $$x/R:100 \to 110$$ and `ydata` is the value of the potential in this region. Lets obtain `xdata` and `ydata`.

~~~
x_data = np.linspace(100, 110, 100)
y_data = np.array([potential(x_R) for x_R in x_data])
~~~
{: .language-python}

Now we are ready for curve fitting. Here we go over the scipy curve fitting model: documentation can be found [here](https://docs.scipy.org/doc/scipy/reference/generated/scipy.optimize.curve_fit.html). For basic use, the function is of the form `curve_fit(f, xdata, ydata, p0)`.

* `f`: The function we are trying to ''fit''. By ''fit'' we mean the function has a domain $$x \in \mathbb{R}$$ but also has some constants $$c_1, c_2$$, ... that specify the shape of the function. For example, the function $$f(x; c_1, c_2) = c_1 x + c_2 x^2$$ takes in all real numbers for $$x$$ and its shape is specified by $$c_1$$ and $$c_2$$. 
* `x_data` and `y_data`: these correspond to the points we want to fit the function to. The goal is to choose $$c_1$$ and $$c_2$$ such that $$f(x_{data}; c_1, c_2) \approx y_{data}$$. In our example we have a function we want to fit $$f(x;a)$$ and the `x_data` are values of $$x$$ in $$(100R,110R)$$ and and `y_data` is $$V(x)$$. 
* `p0` Are initial *guesses* of what we believe $$c_1$$ and $$c_2$$ should be. If our initial guesses aren't close enough, then scipy will never converge to the optimal parameters. In our case we need to pick a "good enough" initial guess for $$a$$


Lets code up our curve_fit:

~~~
from scipy.optimize import curve_fit
fit_results = curve_fit(f, x_data, y_data, p0=[1])
print(fit_results)
~~~
{: .language-python}

Take a look at the print statement of `print(fit_results)`. You'll see it returns a tuple where the first array contains $$c_1$$, $$c_2$$, ... (in our case we only have one fit parameter $$a$$ so the array is just a length of 1) and the second array contains error information (no need to worry about that in this course). Lets extract the value for $$a$$:

~~~
a_fit = fit_results[0][0]
~~~
{: .language-python}


Lets see how good the fit is:
~~~
plt.plot(x_data, y_data, label='Potential Due To Spring')
plt.plot(x_data, f(x_data, a_fit), label='Best Fit', ls='--')
plt.xlabel('$x/R$', fontsize=14)
plt.ylabel(r'$\left( \frac{4 \pi \epsilon_0 R}{Q} \right)V(x\hat{x})$', fontsize=14)
plt.grid()
plt.legend()
plt.show()
~~~
{: .language-python}

Now lets see how well it does in the region $$x:2R \to 5R$$:

~~~
x_data = np.linspace(2, 5, 100)
y_data = np.array([potential(x_R, 0, 0) for x_R in x_data])

plt.plot(x_data, y_data, label='Potential Due To Spring')
plt.plot(x_data, f(x_data, a_fit), ls='--', label='Best Fit')
plt.xlabel('$x/R$', fontsize=14)
plt.ylabel(r'$\left( \frac{4 \pi \epsilon_0 R}{Q} \right)V(x\hat{x})$', fontsize=14)
plt.grid()
plt.legend()
plt.show()
~~~
{: .language-python}

Not so good in this region! What about the region $$x:200R \to 210R$$?

~~~
x_data = np.linspace(200, 210, 100)
y_data = np.array([potential(x_R, 0, 0) for x_R in x_data])

plt.plot(x_data, y_data, label='Potential Due To Spring')
plt.plot(x_data, f(x_data, a_fit), ls='--', label='Best Fit')
plt.xlabel('$x/R$', fontsize=14)
plt.ylabel(r'$\left( \frac{4 \pi \epsilon_0 R}{Q} \right)V(x\hat{x})$', fontsize=14)
plt.grid()
plt.legend()
plt.show()
~~~
{: .language-python}

Clearly the fit (done in the region $$x:100R \to 110R$$) better extrapolates to the region $$x:200R \to 210R$$ than it does to the region $$x:2R \to 5R$$ which is as to be expected.

> ## Machine Learning and Artificial Intelligence
> In our current problem we are considering a function $$f(x;a)$$; in other words, there is one model parameter $$a$$. This is the same mathematical formulation that is used in machine learning: a **neural network** is of the form $$f(x;a_1,a_2,...)$$ where typically there are millions of $$a$$ values. Machine learning is typically more complicated: for example, in image classification, $$x$$ is now a 3D image (height, width, color) and each $$a_i$$ corresponds to one neuron. In addition, $$f(x;a_1,a_2)$$ is no longer a number but rather a string that describes the image.
{: .challenge}

> Using your fit value for $$a$$, re-express the formula for $$V(x\hat{x})$$. Compare it to the formula for the potential $$V(x \hat{x})$$ for a point charge at the origin. Does this look familiar? Why is this the case? Comment on why, in question 2, the fit better extrapolates to the region $$x:200R \to 210R$$ than to the region $$x:2R \to 5R$$.

Using our fit we have

$$\left( \frac{4 \pi \epsilon_0 R}{Q} \right)V(x\hat{x}) \approx \frac{a}{x/R}$$

or

$$V(x\hat{x}) \approx a\left( \frac{Q}{4 \pi \epsilon_0 x} \right)$$

If you print `a_fit` you should find that it is approximately equal to $$1$$ and thus 

$$V(x\hat{x}) \approx \frac{Q}{4 \pi \epsilon_0 x}$$

which is precisely what the potential would be for a point charge at the origin. The reason why this is the case is because when we're far from the charge source ($$x=100R$$ away) the charge source effectively *looks* like a point charge at the origin.


{% include links.md %}
