---
title: "Tensor Product for Programmers"
date: 2019-04-15
draft: false
categories:
- Math
tags:
- Math
- Algebra
- Tensors
---

The introduction to tensor products and tensor algebras is often riddled with rigor, in which a mathematician would 
delight but a programmer would despair. I find myself in the intersection of these camps and while I appreciate 
notation, a simpler introduction is possible using functional programming concepts. 

Tensors are defined and introduced in two equivalent ways. The first way, called the "expansion coefficient" (or array) 
style of introducing tensors relies on many indices and iterates over the n dimensions of some array (n-dimensional 
generalization of a matrix). I have found this approach to be overly cluttered; missing the forest for the trees. 
Instead, I prefer the second way of defining tensors, namely as "multilinear maps". [^1] [^2]
This technique focuses on functions and interfaces, as opposed to components, and will be the chosen method of explaining 
below. 

Before we begin, a brief note about preferences between the "coefficient" and "multilinear" approaches. The latter way 
has found greater resonance with mathematicians, while programmers often find the former more comforting. I believe 
this is caused by an over-reliance on data structures as atomic units of understanding. True, it is natural as a 
developer to ask "but what _is_ the object"; however, using more functional-programming style of thought, the 
multilinear map approach is actually simpler! No need to keep track of various coefficients on various axes of some 
imaginary n-dimensional array (leave that to `numpy`). In the below, I outline a functional-programming style analogy 
for tensors, and the tensor product. Thought the below snippets are in python, some details are left to the imagination 
(i.e. this code is not a script).


## Setting the Stage
Before we get to define tensors, we need to briefly define a few building blocks. First, there is a field $C$, commonly 
the reals $\mathbb{R}$, occasionally the complex numbers $\mathbb{C}$. Members of $C$ are called _scalars_ and are 
represented in the code by the type `scalar`. Second, there are vector spaces $V$ over this field, with the usual 
properties of closure under addition and scalar multiplication. Elements of $V$ are of type `vector`. Note, I am not 
specifying a vector as a tuple of scalars - though that is a valid vector space, there are others based on non-tuple 
like entities, like the vector space of square-integrable functions! The last piece of machinery is the dual space $V^{\*}$. 
If you're not familiar with the dual space, it is essentially the set of all linear functions that take 1 vector and spit 
out a scalar (specifically $V^* = \{f: V\rightarrow C\}$ where $f$ is linear). Elements of $V^*$ are of type `dual`.

## Some dual vectors
Recall that dual vectors are functions that take a vector and return a scalar. Let $f, g \in V^*$. In code:

```python
def f(v: vector) -> scalar:
    pass

def g(w: vector) -> scalar:
    pass
```


## Tensor Product 
Now let's define the tensor product of $f$ and $g$ as $h = f \otimes g$. What this amounts to, is combining the 
functional interface into a new, single tensor (function), that curries to the functions it was made from! Specifically:
 
```python
def h(a: vector, b: vector) -> scalar:
    return f(a) * g(b)
```

In some sense, the tensor (or outer) product is like a concatenation operation, that joins functions together, using 
the superset of arguments, and passing those arguments back to the original functions returning a scalar! This definition 
is easy!


[^1]: N. Jeevanjee, An Introduction to Tensors and Group Theory for Physicists (Springer Science+Business Media, New York, NY, 2015).
[^2]: S. Roman, Advanced Linear Algebra, 3rd ed (Springer, New York, 2007).