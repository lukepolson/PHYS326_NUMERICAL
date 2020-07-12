---
title: "Tutorial 1"
teaching: 0
exercises: 0
questions:
- "Key question (FIXME)"
objectives:
- "First learning objective. (FIXME)"
keypoints:
- "First key point. Brief Answer to questions. (FIXME)"
---

> ## Question 1
> Consider the function  $$f(x,y) = e^{-y}\sin(x)$$ Plot 
>
> 1. $$f(x, 1)$$ vs. $$x$$ for $$x$$ in the interval $$(-4\pi, 4\pi)$$
> 2. $$f(\pi/2, y)$$ vs. $$y$$ for $$y$$ in the interval $$(-3, 3)$$
{: .callout}

The key is to first define a python function which represents $$f(x,y)$$. We will need pythons numerical library and plotting library, so we import `numpy` and `matplotlib.pyplot`

~~~
import numpy as np
import matplotlib.pyplot as plt
~~~
{: .language-python}

Since we imported numpy "as" `np` and matplotlib.pyplot "as" plt, `np.____` represents a function from the library of numpy. We will need to use the sine and exponential functions from numpy. First we define a python function that is $$f(x,y)$$.

~~~
def func(x,y):
    return np.exp(-y)*np.sin(x)
~~~
{: .language-python}

Recall that numpy is quite versatile; the function arguments `x` and `y` need not be floats: they can also be numpy arrays. In particular

* If both $$x$$ and $$y$$ are numbers, the return of `func` will also be a number
* If one of $$x$$ and $$y$$ is a number and the other is a numpy array, then the return will be an array
* If both $$x$$ and $$y$$ are numpy arrays, then they **must** be the same size and the return will be an array.

Now we define an array of values from $$-4\pi$$ to $$4\pi$$. The easiest way to do this is to use `np.linspace` or `np.arange`. They work slightly differently:

~~~
# Option 1
x = np.linspace(-4*np.pi, 4*np.pi, 1000)
# Option 2
x = np.arange(-4*np.pi, 4*np.pi, 0.01)
~~~
{: .language-python}

The first option creates an array of 1000 equally spaced $$x$$ points from $$-4\pi$$ to $$4\pi$$. The second option creates $$x$$ points with a spacing of $$0.01$$ to go from $$-4\pi$$ to $$4\pi$$.  Now we can plug in the array to our numpy function.

~~~
f = func(x,1)
~~~
{: .language-python}




{% include links.md %}

