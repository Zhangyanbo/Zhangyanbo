---
title: From Infinite Depth to One Step Away
summary: How the Implicit Function Theorem Offers a Shortcut for Neural Networks
date: 2025-08-21
authors:
  - admin
  - gemini
math: true
tags:
  - Machine Learning
  - Reading Note
  - Optimization
image:
---

When we talk about deep learning, the first image that comes to mind is likely a neural network stacked with layers. From AlexNet's 8 layers to ResNet's 152 layers, and now Transformer variants with thousands of layers, a model's "depth" seems to have become synonymous with its power. This "stacking" philosophy is simple and intuitive: just like building a skyscraper, the more floors you have, the more complex the functions and the wider the view.

But a skyscraper cannot be built to any height we desire. Problems like vanishing/exploding gradients and immense memory costs (for storing activations of every layer during backpropagation) are like clouds hanging over our tall buildings. Thus, researchers began to consider an interesting question: what if, instead of building new floors layer by layer, we used the same blueprint to repeatedly refine the same floor?

This is the idea behind weight sharing or recurrent networks. In this mode, the input signal is repeatedly iterated within the same module, constantly self-updating. If we iterate a sufficient number of times, the signal may eventually reach a "perfected" and stable state—a state where it no longer changes. This is known as a fixed point or equilibrium. This fixed point contains all the information that the model can extract at "infinite depth".

This idea is very appealing, as it suggests we might be able to use a very small model to simulate an infinitely deep network through multiple iterations. But new problems arise:

Forward propagation: Do we really have to iterate hundreds or thousands of times to find that fixed point? That would be terribly inefficient.
Backpropagation: If we really did iterate 1000 times, wouldn't backpropagation require storing 1000 intermediate states? The memory cost would be a catastrophe, even worse than stacking 1000 different layers.
So, is there a way for us to have a "god's eye view," to skip directly to that final fixed point and, during training, calculate the gradient in just one step of backpropagation? The answer is yes, and the key lies in a powerful tool from mathematics: the **Implicit Function Theorem (IFT)**.

## A New Beginning: A Story of "Repeated Refinement'
To understand this process, let's create a vivid analogy. Imagine we are a master sculptor, and the input data $x$ in our hands is a raw block of clay. Our goal is to sculpt it into a magnificent masterpiece (the final output). Our neural network $f_\theta$ is a set of sculpting tools and techniques, where the parameters $\theta$ represent the sharpness of the tools and the mastery of the techniques.

A traditional deep network is like an assembly line with dozens of different processes, where each process (a network layer) uses a different tool to shape the clay. The iterative model we are discussing, however, is more like a seasoned craftsman who uses only their favorite set of tools, repeatedly polishing and meticulously refining the same block of clay.

We use $z_i$ to denote the state of the clay after the $i$-th refinement. The entire process can be written as:
$$
z_{i+1} = f_\theta(z_i, x)
$$
Here, we also use the original clay information $x$ as a reference for each refinement to ensure we don't stray from the core subject. After countless refinements, the state of the clay will become more and more perfect, eventually reaching a state where "one more cut would be too much, and one less would be insufficient." This is the fixed point $z^*$. In this state, our sculpting techniques can no longer improve it, which means:
$$
z^* = f_\theta(z^*, x)
$$
This is the final output we have been dreaming of. Now, our core problem becomes: how do we efficiently find $z^*$, and how do we adjust our techniques $\theta$ based on the quality of $z^*$?

## The Naive Approach: "Rewinding" Step by Step

What is the most straightforward training method? It's to dutifully record the entire process of every refinement.

Forward propagation: Starting from $z_0$ (which could be the input $x$), iterate $N$ times to get $z_1, z_2, \dots, z_N$. We assume $z_N$ is already very close to the fixed point $z^*$.
Backpropagation: Calculate the discrepancy (loss function $\mathcal{L}$) between the final $z_N$ and our vision of a perfect masterpiece (the true label $y$). Then, like rewinding a movie, we start from $z_N$ and propagate the gradient back step by step to $z_{N-1}$, then to $z_{N-2}$, and so on, all the way back to $z_0$. This process is the famous Backpropagation Through Time (BPTT).
The fatal flaw of this method is that to "rewind," we must store every frame of the process ($z_1, \dots, z_N$). When the number of iterations $N$ is very large, the memory consumption becomes astronomical. It's like a sculptor having to remember every detail of the hundreds of cuts they just made to improve their technique, which is clearly unrealistic.

## A Stroke of Genius: The "Shortcut" of the Implicit Function Theorem

Do we really need to care about the intermediate process? Not at all. We only care about the relationship between the final masterpiece $z^*$ and our techniques $\theta$. In other words, we want to know: **if my sculpting tool (parameter $\theta$) becomes slightly sharper, what will happen to the final masterpiece (the fixed point $z^*$)?**

The Implicit Function Theorem (IFT) is the perfect tool to answer this question.
Let's look at the defining equation of the fixed point again:
$$
z^* - f_\theta(z^*, x) = 0
$$
This equation defines an implicit relationship between $\theta$ and $z^*$. It does not explicitly state how $z^*$ is a function of $\theta$, like $z^* = g(\theta)$, but rather links them through an equilibrium equation. IFT tells us that even if we don't know this explicit function $g(\cdot)$, we can still directly calculate its derivative $\cfrac{\text d z^*}{\text d \theta}$!

The derivation is a bit mathematical, but the idea is very intuitive. We assume the parameter $\theta$ undergoes an infinitesimal change $\text d\theta$, which causes the fixed point to also undergo an infinitesimal change $\text dz^*$. But even after the change, the new fixed point $z^*+\text dz^*$ and the new parameter $\theta+\text d\theta$ must still satisfy the equilibrium equation. We take the total derivative of both sides of the equilibrium equation with respect to $\theta$:
$$
\frac{\text d}{\text d\theta} \left( z^* - f_\theta(z^*, x) \right) = \frac{\text d}{\text d\theta}(0)
$$
Using the chain rule to expand the left side, we get:
$$
\frac{\text d z^*}{\text d \theta} - \left( \frac{\partial f_\theta}{\partial z^*} \frac{\text d z^*}{\text d \theta} + \frac{\partial f_\theta}{\partial \theta} \right) = 0
$$
This is a linear equation for the derivative we want, $\cfrac{\text d z^*}{\text d \theta}$! Let's rearrange it:
$$
\left( I - \frac{\partial f_\theta}{\partial z^*} \right) \frac{\text d z^*}{\text d \theta} = \frac{\partial f_\theta}{\partial \theta}
$$
Here, $I$ is the identity matrix, and $\cfrac{\partial f_\theta}{\partial z^*}$ is the Jacobian matrix of the function $f_\theta$ with respect to its first input (the state of the previous step), which we'll denote as $J_f$. Solving this equation, we get:
$$
\frac{\text d z^*}{\text d \theta} = \left( I - J_f \right)^{-1} \frac{\partial f_\theta}{\partial \theta}
$$
But we're not done yet. We are ultimately interested in the gradient of the loss function $\mathcal{L}$ with respect to the parameter $\theta$. Using the chain rule one more time:
$$
\frac{\partial \mathcal{L}}{\partial \theta} = \frac{\partial \mathcal{L}}{\partial z^*} \frac{\text d z^*}{\text d \theta} = \frac{\partial \mathcal{L}}{\partial z^*} \left( I - J_f \right)^{-1} \frac{\partial f_\theta}{\partial \theta}
$$
Look! This is the magical "one-step" gradient formula!
Let's analyze the extraordinary implications of this formula:

Goodbye BPTT: The calculation of this formula only depends on the final fixed point $z^*$, completely bypassing the intermediate iterative steps $z_1, z_2, \dots$.

Constant Memory: We only need to store the state of $z^*$ to calculate the full gradient. Memory consumption plummets from $\mathcal{O}(N)$ to a constant $\mathcal{O}(1)$!

Forward/Backward Decoupling: Forward propagation (how to find $z^*$) and backpropagation (how to use $z^*$ to calculate the gradient) are separated. For the forward pass, we can use any efficient root-finding algorithm (like Broyden's method), while the backward pass directly applies this formula.

This is like we've invented a magic trick to directly analyze the final masterpiece $z^*$ and instantly deduce how our technique $\theta$ should be improved, without needing to recall the entire sculpting process. This is the core idea of **Deep Equilibrium Models (DEQ)**.

## Another Obstacle: Matrix Inversion

There's no free lunch. While IFT helps us bypass the memory hell of BPTT, it leaves us with a new challenge: the inverse of that huge Jacobian matrix $(I - J_f)^{-1}$ in the formula.

In a neural network, the dimension of $z^*$ can be in the millions, which means $J_f$ is a million-by-million matrix. Directly calculating its inverse has a computational complexity of $\mathcal{O}(d^3)$ ($d$ is the dimension of $z^*$), which is computationally infeasible.

What do we do? Fortunately, we usually don't need to compute the full inverse matrix. What we truly need is the product of the vector $\cfrac{\partial \mathcal{L}}{\partial z^*}$ and the matrix $(I-J_f)^{-1}$. This is a classic linear system problem that can be efficiently solved using iterative methods like the Conjugate Gradient method.

However, researchers have found an even more concise path, which is evident in papers like "On Training Implicit Models" and "Hierarchical Reasoning Model"—approximation.

### The Shortcut to the Shortcut: Neumann Series Approximation

Remember the geometric series from high school math? When the common ratio $|q|<1$, we have $\sum_{i=0}^\infty q^i = \cfrac{1}{1-q}$. This idea can be extended to matrices, giving us the Neumann series:
If the spectral radius of matrix $A$ is less than $1$, then:
$$
(I - A)^{-1} = I + A + A^2 + A^3 + \dots
$$
Applying this to our problem, let $A = J_f$, and we get:
$$
(I - J_f)^{-1} \approx I + J_f + J_f^2 + \dots + J_f^{k-1}
$$
We can just take the first $k$ terms of this series to approximate that pesky matrix inverse!

When $k=1$, $(I - J_f)^{-1} \approx I$. This is called the one-step gradient. The gradient formula simplifies to $\cfrac{\partial \mathcal{L}}{\partial \theta} \approx \cfrac{\partial \mathcal{L}}{\partial z^*} \cfrac{\partial f_\theta}{\partial \theta}$. This is the fastest but possibly the least accurate approximation.
When $k > 1$, what we get is called the phantom gradient. It offers a flexible trade-off between computational cost and gradient accuracy. The larger $k$ is, the more accurate the approximation, but the greater the computational load.
In practice, we don't even need to explicitly calculate the powers of $J_f$; instead, we can efficiently compute the final result through $k$ iterations of Jacobian-vector products. This makes the entire backpropagation process incredibly lightweight.

## The Forward Process

We already know that for the fixed point $z^* = f_\theta(z^*, x)$, we can derive an analytical gradient expression using the Implicit Function Theorem (IFT). However, in practice, we prefer to leverage the convenience of automatic differentiation frameworks like PyTorch, rather than manually implementing complex gradient calculations. This leads to a core question: since we already have the target gradient formula, can we design an equivalent forward propagation process that allows the automatic differentiation engine to "automatically" compute the gradient we want during backpropagation?

The answer is yes. To understand this, we first need to recall the essence of automatic differentiation. Autodiff does not perform symbolic differentiation; rather, it applies the chain rule to a forward computation that has already occurred. During the forward pass, it records each operation, building a Computational Graph. During the backward pass, it traces this graph backward, mechanically applying the chain rule step by step. Therefore, the structure of the forward computational graph completely determines what gradient is calculated during backpropagation.

Our task is to "play to its strengths" and "fabricate" a corresponding forward computational graph for our approximate gradient formula.

### One-Step Gradient

Let's start with the simplest one-step gradient approximation. This approximation comes from truncating the Neumann series $(I - J_f)^{-1} = I + J_f + J_f^2 + \dots$ to its first term, i.e., $(I - J_f)^{-1} \approx I$. Substituting this into the full IFT gradient formula $\cfrac{\partial \mathcal{L}}{\partial \theta} = \cfrac{\partial \mathcal{L}}{\partial z^*} (I - J_f)^{-1} \cfrac{\partial f_\theta}{\partial \theta}$, our target gradient becomes:
$$
\frac{\partial \mathcal{L}}{\partial \theta} \approx \frac{\partial \mathcal{L}}{\partial z^*} \frac{\partial f_\theta}{\partial \theta}
$$
Now let's analyze this gradient. Based on the chain rule, it is actually equivalent to calculating the gradient for a function $g(\theta) = f_\theta(z^*, x)$, while a core requirement is that, during the differentiation, **$z^*$ must be treated as a constant independent of $\theta$**.

How do we make the autodiff engine treat $z^*$ as a constant? The method is simple: turn off gradient tracking while computing $z^*$. This leads to a clear two-stage forward construction method, a technique clearly demonstrated in the pseudocode of the HRM model.

```python
# Goal: Construct a forward process whose autodiff gradient is equivalent to the one-step gradient

def forward_one_step(x, params):
    # Stage 1: In a no-grad environment, find the fixed point z* via an iterative solver
    # This operation will not be recorded in the computational graph
    with torch.no_grad():
        z_star = solver(func, x, params)
    # Stage 2: In a normal, gradient-tracking environment,
    # perform one transformation on z_star (now treated as a constant input)
    z_final = func(z_star, x, params)

    return z_final
```

The computational graph built by this forward process is very small. It only records the single transformation from `z_star` to `z_final`. Since `z_star` was calculated in a `no_grad` environment, it contains no history information related to `params`. Therefore, when the autodiff engine backpropagates, the gradient will only flow through this last step, perfectly calculating the gradient we want.

### Unrolled Phantom Gradient (UPG)

Although the one-step gradient is simple to implement, its approximation can be quite coarse. To obtain a more accurate gradient estimate, we can use the Unrolled Phantom Gradient (UPG), which corresponds to a longer truncation of the Neumann series, thus striking a better balance between gradient accuracy and computational cost.

Mathematically, the UPG's gradient is defined as the result of differentiating a sequence that starts from the fixed point $z^*$ and iterates $k$ times, all while strictly adhering to one core premise: treating the starting point $z^*$ as a constant independent of the model parameters $\theta$.

To clearly see what this gradient is, let's first construct this computational sequence of length $k$. Let the starting point of the sequence be $z^{(0)} = z^*$, and each subsequent term is updated via the function $f_\theta$:
$$
z^{(i)} = f_\theta(z^{(i-1)}, x) \quad \text{for} \quad i = 1, 2, \dots, k
$$
The final loss function $\mathcal{L}$ acts on the last term of this sequence, $z^{(k)}$. The gradient we want to solve is $\cfrac{d\mathcal{L}}{d\theta}$. According to the chain rule, we have:
$$
\frac{\text d\mathcal{L}}{\text d\theta} = \frac{\partial \mathcal{L}}{\partial z^{(k)}} \frac{\text d z^{(k)}}{\text d\theta}
$$
The key here is to compute $\cfrac{\text d z^{(k)}}{\text d\theta}$. We take the total derivative of both sides of the iteration formula $z^{(k)} = f_\theta(z^{(k-1)}, x)$ with respect to $\theta$:
$$
\frac{\text d z^{(k)}}{\text d\theta} = \frac{\partial f_\theta}{\partial z^{(k-1)}} \frac{\text d z^{(k-1)}}{\text d\theta} + \frac{\partial f_\theta}{\partial \theta}
$$
This is a recursive formula. We can repeatedly unroll it. To simplify notation, let's denote $J_f^{(i-1)} = \cfrac{\partial f_\theta}{\partial z^{(i-1)}}$ as the Jacobian matrix at the point $z^{(i-1)}$. Unrolling it once, we get:
$$
\frac{\text d z^{(k)}}{\text d\theta} = J_f^{(k-1)} \left( \frac{\partial f_\theta}{\partial z^{(k-2)}} \frac{\text d z^{(k-2)}}{\text d\theta} + \frac{\partial f_\theta}{\partial \theta} \right) + \frac{\partial f_\theta}{\partial \theta} = J_f^{(k-1)} J_f^{(k-2)} \frac{\text d z^{(k-2)}}{\text d\theta} + J_f^{(k-1)} \frac{\partial f_\theta}{\partial \theta} + \frac{\partial f_\theta}{\partial \theta}
$$
This process continues until we unroll to the starting point $z^{(0)}$. At this point, we apply our core premise: $z^{(0)} = z^*$ is treated as a constant, so its derivative $\cfrac{\text d z^{(0)}}{\text d\theta} = 0$. This makes the end of the recursion a zero term, and the fully unrolled expression only contains terms related to $\cfrac{\partial f_\theta}{\partial \theta}$.

This unrolled gradient form is precisely what the autodiff engine calculates via the chain rule when backpropagating through a computational graph containing $k$ function calls. Therefore, to make the autodiff engine compute the UPG for us, we need to construct a forward process that corresponds perfectly to it. This process must satisfy two conditions: 1) The starting point of the sequence is the true fixed point $z^*$; 2) When calculating the gradient, this starting point has no gradient history with respect to $\theta$. This again leads us to use the `no_grad` trick, a method clearly discussed and implemented in "On Training Implicit Models."
```python
# Goal: Construct a forward process whose autodiff gradient is equivalent to UPG

def forward_upg(x, params, k=5):
    # Stage 1: Find the fixed point z* in a no-grad environment
    # This step shields the solver process, ensuring z_star has no "history" on the computational graph
    with torch.no_grad():
    		z_star = solver(func, x, params)
    # Stage 2: Using z_star as the starting point, perform k iterations
    # This loop's operations will be fully recorded by autodiff, forming a computational graph of length k
    z_k_steps = z_star
    for _ in range(k):
        z_k_steps = func(z_k_steps, x, params)

    return z_k_steps
```

The forward process built by this code perfectly aligns with our derivation. The solver runs in a `no_grad` environment, ensuring `z_star` is a constant in the computational graph sense. The subsequent `for` loop then builds a computational chain of length $k$ in a normal gradient-tracking environment. When the autodiff engine backpropagates through this, the result of the chain rule operation is fully consistent with our manually derived UPG gradient expression, thus achieving our goal efficiently and accurately.

## Summary
This article started with the alluring prospect of "infinitely deep" networks and embarked on a journey to find an efficient training method. We began with the most intuitive idea of recurrent iteration, only to quickly encounter the dual dilemma of memory and computation brought about by Backpropagation Through Time (BPTT). This forced us to consider a more fundamental question: do we have to completely "rewind" the entire computation to effectively optimize the model?

The answer is no. The Implicit Function Theorem (IFT) acts as a key, opening the door to a new world. It elegantly reveals that we don't need to care how the model arrived at "equilibrium'; by focusing only on the final "fixed point" state, we can precisely derive the gradient. This profound insight reduces the memory cost of backpropagation from $\mathcal O(T)$, which is dependent on the number of iterations, to a constant $\mathcal O(1)$, fundamentally solving the biggest bottleneck in training deep recurrent models.

Of course, theoretical perfection always meets new challenges in practice—the problem of inverting the huge Jacobian matrix. But this did not stop our progress; instead, it led to more ingeniously engineered solutions. Whether it's the straightforward "one-step gradient" or the more refined "unrolled phantom gradient" (UPG), we've learned how to "fabricate" a forward process equivalent to complex mathematical derivations by manipulating the autodiff engine's computational graph. These "tricks" not only bring the theory to life but also demonstrate the powerful flexibility of modern deep learning frameworks.

Implicit models, represented by DEQ and HRM, show us a possible paradigm that goes beyond the traditional "layer-by-layer stacking." Although they require an iterative solver for the forward pass, unlike simple feedforward networks, they trade constant memory overhead for the potential to simulate "infinite depth" computation. This makes it possible to handle tasks that require long, complex reasoning, which was previously unimaginable.

Perhaps this reveals a new possibility for deep learning: true "depth" may not be about how tall a building we construct, but rather our ability to allow a system to converge to a profound and self-consistent "equilibrium" through repeated self-reflection and iteration.