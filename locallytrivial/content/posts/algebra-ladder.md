---
title: "Algebra Ladder"
date: 2020-03-15
draft: false
categories:
- Math
tags:
- Math
- Algebra
- Group Theory
---

I first encountered a diagram of algebraic structures at the end of Jeevanjee's second chapter,
"Vector Spaces", which elegantly summarizes the high-level differences in structure between sets,
vector spaces, and inner product spaces. [^1]

This diagram was immensely helpful to me, in that it helped show the relationships between various
commonly used objects in mathematical physics. As I've encountered new structures, I've attempted
to augment this map along two dimensions: a _structure_ dimension that aims to measure the number
of attributes an algebraic object has, and a _specificity_ dimension which measures the amount
of constraints placed on each attribute.

For instance, a Magma has more structure than a set, because a new attribute - a binary operator - has
been added. A group, though, is roughly similar in structure to a magma, but has more properties of
the binary operator specified, such as associativity, inverses, and identity, which make it more
specific (and the magma more general). [^2]

{{< image src="/images/algebra-ladder.png" title="Algebra Ladder" caption="Relationship of various algebraic structures." >}}

The diagram above aims to show how an algebra is constructed from a set, though admittedly omits
several algebraic structures along the way. I've attempted to include the most primary objects used or
seen in mathematical physics. I should also note, this diagram is intended as a quick-reference, and
isn't a substitute for opening Hungerford! [^3]

[^1]: N. Jeevanjee, An Introduction to Tensors and Group Theory for Physicists (Springer Science+Business Media, New York, NY, 2015).
[^2]: S. Roman, Advanced Linear Algebra, 3rd ed (Springer, New York, 2007).
[^3]: T. W. Hungerford, Algebra, Corr. 8th print (Springer, New York, 2003).

