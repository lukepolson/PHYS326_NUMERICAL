---
title: "Tutorial 2"
teaching: 0
exercises: 0
questions:
- "Question 1"
- "Question 2"
- "Question 3"
objectives:
- "Use NumPy to get domain and range of functions for plotting"
- "Use matplotlib to plot simple functions"
- "Use scipy to evaluate basic integrals"
- "Use mathematical tricks to make complicated functions plotable numerically"
keypoints:
- "NumPy and matplotlib are the standard python libraries you will use in this course"
- "SciPy has methods for evaluating integrals numerically"
- "Through clever use of algebra, one can plot complicated functions with undefined variables"
---

Consider a uniform line-charge distribution parametetrized by $$\theta$$ with total charge $$Q$$ located at

$$\vec{r} = (R\cos(2\theta), R\sin(2\theta), R\theta)$$

for $-\pi \leq \theta \leq \pi$. This represents a ''spring'' of charge of with total $Q$. 

## Question 1
> Plot the potential along the $x$ axis for $x:2R \to 10R$.

It can be shown that

$$V(x\hat{x}) = \frac{Q}{8 \pi^2 \epsilon_0} \int_{0}^{2 \pi} \frac{d\theta}{\sqrt{(x-R\cos\theta)^2+R^2\sin^2\theta + \theta^2}} $$

However, for plotting, we need to plot a dimensionless quantity. We can rearange the integral to be dimensionless and obtain the following expression:

$$\left( \frac{8 \pi^2 \epsilon_0 R}{Q} \right)V(x\hat{x}) = \int_{0}^{2\pi}\frac{1}{\sqrt{(x/R-\cos(2\theta'))^2+\sin(2\theta')^2+\theta'^2}}d\theta'$$

Now we define the integrand and the "potential" (by "potential" I really mean the dimensionless $$\left( \frac{8 \pi^2 \epsilon_0 R}{Q} \right)V(x\hat{x})$$).

~~~
def integrand(theta, x_R):
    return 1/np.sqrt((x_R-np.cos(2*theta))**2+np.sin(2*theta)**2+theta**2)
def potential(x_R):
    return quad(integrand, 0, 2*np.pi, args=[x_R])[0]
~~~
{: .language-python}

Now we evaluate the potential for many different values of $$x$$ from $$x/R: 2 \to 10$$.

~~~
x_Rs = np.linspace(2,10,1000)
potentials = np.vectorize(potential)(x_R)
~~~
{: .language-python}

And now we can plot like we've done in tutorial 1.

~~~
plt.plot(x_Rs, potentials)
plt.xlabel('$x/R$', fontsize=14)
plt.ylabel(r'$\left( \frac{8 \pi^2 \epsilon_0 R}{Q} \right)V(x\hat{x})$', fontsize=14)
plt.grid()
plt.title('Potential from Charged Spring')
plt.show()
~~~
{: .language-python}

# Question 2
> For the region $x:100R \to 110R$, fit the potential to the curve $f(x;a)=\frac{a}{x/R}$. What value of $a$ do you get? Then, using this value of $a$, plot $V(x\hat{x})$ and $f(x;a)$ on the same plot in 
> * the region $x:2R \to 5R$.
> * the region $x:200R \to 210R$






{% include links.md %}
