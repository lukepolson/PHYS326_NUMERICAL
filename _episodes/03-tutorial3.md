---
title: "Tutorial 3"
teaching: 30
exercises: 30
questions:
- "Part 1: Special Functions in Python"
- "Part 2: Bar Plots in Python"
objectives:
- "Learn how to import special functions using scipy"
- "Learn how to make basic barplots in python"
keypoints:
- "Whenever you need a special function in python, it usually can be found in the scipy library"
- "Barplots are generally easy to make in python, but can often be a little finnicky"
---

# Part 1: Special Functions in Python

Throughout your studies in physics, many special functions will be encountered. These include things like the legendre, laguerre, and hermite polynomials. While it is possible to look these up in a table and copy them into python, scipy has these polynomials already coded up. In this tutorial we will look at the hermite polynomials.


> Recall from PHYS 215 that wavefunction for the harmonic oscillator can be written as 
> $$\psi_n(x) = \left(\frac{m\omega}{\pi \hbar}\right)^{1/4}\frac{1}{\sqrt{2^nn!}}H_n(\xi)e^{-\xi^2/2} $$
> where $$\xi=\sqrt{m\omega/\hbar} x$$ and $$H_n$$ are the Hermite polynomials. Plot $$\psi_0$$, $$\psi_1$$, $$\psi_2$$, $$\psi_4$$, and $$\psi_8$$ as a function of $$\xi$$.

First we import the hermite polynomials from scipy

~~~
from scipy.special import hermite
~~~
{: .language-python}

This function takes in a parameter $n$ and returns a `poly1d` object.

~~~
hermite_2 = hermite(2)
~~~
{: .language-python}

We can then evaluate the polynomial for arbitrary $$\xi$$

~~~
hermite_2(10)
~~~
{: .language-python}

Alternative, we can do this as follows:

~~~
hermite(2)(10)
~~~
{: .language-python}

where "2" defines $$n$$ and "10" specifies the value of $$\xi$$ in $$H_n(\xi)$$. Now lets plot the wave functions. First we define a function that gives us the wave functions (as a function of $$\xi$$ and $$n$$). Note that to make everything dimensionless, the function really represents $$\left(\frac{\pi \hbar}{m\omega}\right)^{1/4} \psi_n(x)$$.


~~~
def psi(xi, n):
    return (1/np.sqrt(2**n * factorial(n))) * hermite(n)(xi) * np.exp(-xi**2 /2)

xi = np.linspace(-10, 10, 100)
psi0 = psi(xi, 0)
psi1 = psi(xi, 1)
psi2 = psi(xi, 2)
~~~
{: .language-python}

Now plot

~~~
plt.plot(xi, psi0, label='$n=0$')
plt.plot(xi, psi1, label='$n=1$')
plt.plot(xi, psi2, label='$n=2$')
plt.xlabel(r'$\sqrt{m\omega/\hbar} x$')
plt.ylabel(r'$\left(\frac{\pi \hbar}{m\omega}\right)^{1/4} \psi_n(x)$')
plt.title('Wave Functions of the Harmonic Oscillator')
plt.legend()
~~~
{: .language-python}

# Part 2: Bar Plots in Python

For this part of the tutorial, we will consider example 3.4 in Griffiths E&M: suppose, however, that $$b=2a$$ so that the potential everywhere becomes 

$$V(x,y) = \frac{4V_0}{\pi} \sum_{n=1,3,5,...} \frac{1}{n}\frac{\cosh(n \pi x/a)}{\cosh(2 n \pi)} \sin(n \pi y / a)  $$

we can define the nth term in this sum as 

$$V_n(x,y) = \frac{4V_0}{\pi} \frac{1}{n}\frac{\cosh(n \pi x/a)}{\cosh(2 n \pi)} \sin(n \pi y / a) $$

> Consider the point $$\vec{r}=(a,a/3,0)$$. Make a bar plot of $$V_n$$ vs. $$n$$ for $$n$$ in the range 1 to 33 to show how quickly the terms decay. Use a semilogy axis and only consider odd $$n$$ (as even $$n$$ terms are equal to zero).

First define $$V_n$$ and get arratst of $$n$$'s and $$V_n$$'s for odd $$n$$ in the range 1 to 33.

~~~
def Vn(x_a, y_a, n):
    return (1/n)*np.cosh(n*np.pi*x_a)/np.cosh(2*n*np.pi) * np.sin(n * np.pi * y_a)
    
ns = np.arange(1, 33, 2)
Vns = Vn(1, 1/3, ns)
~~~
{: .language-python}

Now we can make a simple bar plot.

~~~
plt.bar(ns, Vns)
plt.semilogy()
plt.xlabel('$n$')
plt.ylabel(r'$V_n\cdot \left(\frac{\pi}{4V_0} \right)$')
plt.minorticks_off()
plt.xticks(ns)
plt.show()
~~~
{: .language-python}

> Now consider two points $\vec{r}_1 = (a,a/2,0)$ and $\vec{r}_2 = (a,a/3,0)$. Plot $V_n$ vs. $n$ with the bars next to eachother.

This is a little finicky in python. First lets define out arrays

~~~
ns = np.arange(1, 33, 2)
Vns_r1 = Vn(1, 1/2, ns)
Vns_r2 = Vn(1, 1/3, ns)
~~~
{: .language-python}

In order to plot the bars next to eachother, we need to shift the n's slightly. We also specify `barWidth`: 0.4 works nicely for this plot but you may need to adjust this value for plots you make. You should try changing `barWidth` and see how it affects this plot.

~~~
barWidth = 0.4
ns1 = ns-barWidth/2
ns2 = ns+barWidth/2

plt.figure(figsize=(10,5))
plt.title('Expansion terms for Griffiths Example 3.4')
plt.grid()
plt.bar(ns1, Vns_r1, width=barWidth, label=r'$\vec{r}=(a,a/2,0)$')
plt.bar(ns2, Vns_r2, width=barWidth, label=r'$\vec{r}=(a,a/3,0)$')
plt.xlabel('Term $n$ in Expansion')
plt.ylabel(r'$V_n(\vec{r})\cdot \left(\frac{\pi}{4V_0} \right)$')
plt.semilogy()
plt.minorticks_off()
plt.xticks(ns)
plt.legend(loc='upper right', fontsize=14)
plt.show()
~~~
{: .language-python}

Note that the terms decay **very** quickly, but differently depending on the point you are situated at.










