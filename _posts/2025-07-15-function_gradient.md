---
title: "Why does the Gradient points towards the Steepest Ascent?"
date: 2025-07-15
permalink: /posts/2025/07/gradient_direction/
tags:
  - gradient
  - math
  - calculus
---
*Prerequisites*: A tiny bit of multivariable calculus, including partial derivatives. A bit of one-dimensional calculus, in particular derivatives and the chain rule.

## 1.Definition of the Gradient

The **gradient** of a function $$f:\mathbb{R}^n\to \mathbb{R}$$ is a vector made up of all the partial derivatives of the function:

$$
\nabla f = \left( \frac{\partial f}{\partial x}, \frac{\partial f}{\partial y}, \ldots \right)
$$

Each component of the gradient tells us how much the function changes if we slightly adjust that variable, keeping all others constant. Collectively, the gradient describes how the function changes as you move in any direction from a given point. Our objective in this post is to show that the gradient points in the direction that the function increases the most.

## 2.Why Is It the Direction of Steepest Ascent? 

To build intuition, let’s consider the 2D case with variables $$x$$ and $$y$$. This setting is easier to visualize, but the same logic extends to higher dimensions.

Suppose you’re **standing on a hill** at the point $$(x_0, y_0) \in \mathbb{R}^2$$, and you want to ascend as rapidly as possible. The elevation at any point is given by a function $$f(x, y)$$.

<img src='/images/gradient3d.png' alt='3D plot of a hill'>

- **2D Level Curves (Contour Plot):** By projecting the surface onto a plane, we can use level curves (contours of constant elevation) to see how the gradient behaves in two dimensions.

<img src='/images/gradient2d.png' alt='2D contour plot with gradient vector'>

The gradient vector at $$(x_0, y_0)$$ not only points in the direction of the steepest ascent—where the function increases most rapidly—but is also always **perpendicular** (orthogonal) to the tangent of any level curve passing through that point. This means that if you walk along a level curve (where the function value stays constant), you are moving in a direction where the gradient has no component; the function does not increase or decrease. In contrast, moving directly in the direction of the gradient gives you the fastest possible increase in the function's value.

To see mathematically why the gradient points in the direction of steepest ascent, let’s compute the **rate of change of the function** as you move away from $$(x_0, y_0)$$ in a particular direction. Suppose you pick a direction given by the unit vector $$\vec{v} = (a, b)$$. If you take a small step of size $$t$$ in this direction, your new position becomes: 

$$(x_0, y_0) + t(a, b) = (x_0+ta, y_0+tb).$$ 

Let’s define a new function:

$$
g(t) = f(x_0 + ta,\, y_0 + tb)
$$

To find out how quickly the elevation changes as you take a small step in the direction of $$\vec{v}$$, we compute the derivative of $$g(t)$$ with respect to $$t$$, using the chain rule:

$$\begin{align}
\frac{dg}{dt} = \frac{\partial f}{\partial x} a + \frac{\partial f}{\partial y} b &= \nabla f \cdot \vec{v}\\ &= \|\nabla f\|\, \|\vec{v}\| \cos(\theta)\\ &= \|\nabla f\| \cos(\theta) 
\end{align}
$$

where $$\theta$$ is the angle between $$\vec{v}$$ and $$\nabla f$$. The rate of change is **maximized when $$\cos(\theta) = 1$$**, meaning $$\theta = 0$$—that is, when $$\vec{v}$$ points exactly in the same direction as the gradient. 

On the other hand, if you go in the **opposite direction**, $$\theta = \pi$$, you get **the steepest descent**.

## 3. Why Does This Matter in Machine Learning?

In machine learning, we often want to minimize a **loss function** $$L(\theta)$$, where $$\theta$$ is a vector of model parameters (like weights in neural nets or coefficients in linear regression).

The gradient $$\nabla L(\theta)$$ tells us how changing the model parameters a little affects the loss function. By what we have done before:

* If you **move in the direction of the gradient**, your loss **increases**.
* If you **move in the opposite direction** (i.e., **negative gradient**), you go **downhill**, minimizing the loss.

This is the basis of **gradient descent**:

$$
\theta_{t+1} = \theta_t - \eta \nabla L(\theta_t)
$$

* You're adjusting your parameters by taking a small step ($$\eta$$ is the learning rate) **in the direction of steepest descent**.
* Over time, this ideally leads you to a **minimum** of the loss function.




