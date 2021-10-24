---
title: "Tensor Type Notation"
date: 2019-04-23
draft: false
categories:
- Math
tags:
- Math
- Algebra
- Tensors
---

## What does type (r, s) mean?
I'd like to discuss the notation of the tensor type, commonly denoted $(r, s)$ as it relates to the tensor product. Specifically, 
the ordering of the vector spaces and dual vector spaces involved in the product. The order matters since tensors are typically 
categorized by the number of vectors and dual vectors they require as arguments. To avoid ambiguity, for a given tensor $T$, I will 
denote the number of vector arguments as $n_v$ and the number of dual vector arguments as $n_d$. 


### Preliminaries
Before we begin, recall that the dual vector space $V^\*$ is defined as the set of linear functionals from $V\rightarrow C$, 
where $C$ is the field over which $V$ is a vector space. Note, the dual space $V^\*$ is defined in terms of the vector space $V$. 
For similar reasons, the topic of dual spaces is introduced after the topic of vector spaces - in other words, **epistemologically, 
the dual space follows the vector space**. I only draw attention to this ordering between the vector space and the dual because 
it informs the aesthetic nature of the notation, as we'll see.


### Conventions
The tensor type $(r, s)$ is used to categories tensors based on the number of vectors and dual vectors they consume. Problem is, 
there is a choice to made between the $r=n_v$ or $r=n_d$ conventions - and this choice isn't made consistently. As I mentioned above, 
the epistemological ordering of vectors and dual vectors is unambiguous; dual vectors follow vectors. It therefore seems natural to 
make the first number $r$ equal the number of vector arguments $n_v$. I'll call this convention the *vector first* convention, or 
"VF" for short. Similarly, I'll call the opposite convention, of using the number of dual vectors $n_d$ as the first number $r$, 
the *dual first* convention, or "DF" for short.


### Usage of Conventions
Some sources use the *VF* convention. [^1] [^2] More sources use the *DF* convention.[^3] [^4] [^5] [^6] [^7] [^8] [^9] [^10] [^11] [^12] 
To explicitly make clear the above conventions, the *VF* convention 
would define a type $(r, s)$ tensor $T$ as

$$T: V_1\times \cdot\cdot\cdot \times V_r \times V^\*_1 \times \cdot\cdot\cdot \times V^\*_s\rightarrow C$$ 

where $V_{i}=V$ and $V^\*_{i}=V^\*$ for all $i$. On the other hand, the *DF* convention would define a type $(r, s)$ tensor $T$ as

$$T: V^\*_1\times \cdot\cdot\cdot \times V^\*_r \times V_1 \times \cdot\cdot\cdot \times V_s\rightarrow C$$ 

where again $V_{I}=V$ and $V^\*_{i}=V^\*$ for all $i$.


## Cartesian vs. Tensor Product Notations
The usage of Cartesian products to define the domain of $T$ is typically used before introducing the tensor product, 
as it is more familiar. In the Cartesian product notation, the *VF* convention places the vector spaces before the dual 
spaces, and in some sense is "aligned" with the way in which the subject is taught. When using the tensor product notation, 
however, this is no longer the case!

Recall the tensor product of two vector spaces $V$ and $W$ is denoted $V\otimes W$ and is the set of all multilinear functions 
from $V^\* \times W^\* \rightarrow C$. Notice how the usage of the tensor product $\otimes$ essentially replaces vector spaces 
with their duals in the Cartesian notation. This "replacement" effect combined with only using the spaces $V$ and $V^\*$ amounts 
to reversing the order of the input spaces the domain. For example, the *VF* convention would define a type $(r, s)$ tensor $T$ 
using the tensor product notation as $$T: V^\*_1\otimes \cdot\cdot\cdot \otimes V^\*_r \otimes V_1 \otimes \cdot\cdot\cdot \otimes 
V_s\rightarrow C$$ For good measure, I will also note that the *DF* convention would define a type $(r, s)$ tensor $T$ using the 
tensor product notation as $$T: V_1\otimes \cdot\cdot\cdot \otimes V_r \otimes V^\*_1 \otimes \cdot\cdot\cdot \otimes 
V^\*_s\rightarrow C$$ Notice that these definitions are equivalent to the previous definitions using Cartesian product 
notation, but that now the vector spaces are written first in what we called the *dual first* convention, not the *vector first* 
convention!

## Why is *DF* convention preferred?
It seems that the $DF$ convention has wider usage and appeal; naturally I wonder why. Since it feels natural to align the 
notation with the epistemological order, in other words, to write $V$ before $V^\*$, then I am forced to conclude that the 
mathematical community, with malice of forethought, prefers to base the definition of a type $(r, s)$ tensor on the tensor 
product notation, rather than the Cartesian notation, since the former requires that $V$ be written before $V^\*$. I personally 
have no objection to the choice, as it seems sensible.

### Parting note on terminology
In the *DF* convention, the number $r$ is often referred to as the *covariant* number and the number $s$ is called the 
*contravariant* number. These terms refer to the number of dual vectors and vectors respectively, since vectors are typically
considered contravariant. Similar terms refer to *lower* and *upper* indices respectively.

I should note that my exploration of these conventions is limited to differential geometry, relativity, and linear algebra texts.

[^1]: N. Jeevanjee, An Introduction to Tensors and Group Theory for Physicists (Springer Science+Business Media, New York, NY, 2015).
[^2]: S. Lang, Linear Algebra, 3rd ed (Springer-Verlag, New York, 1987).
[^3]: L. W. Tu, Differential Geometry: Connections, Curvature, and Characteristic Classes (Springer Science+Business Media, New York, NY, 2017).
[^4]: C. W. Misner, K. S. Thorne, J. A. Wheeler, and D. Kaiser, Gravitation (Princeton University Press, Princeton, N.J, 2017).
[^5]: B. C. Hall, Lie Groups, Lie Algebras, and Representations: An Elementary Introduction, Second edition (Springer, Cham ; New York, 2015).
[^6]: P. Renteln, Manifolds, Tensors, and Forms: An Introduction for Mathematicians and Physicists (Cambridge University Press, Cambridge, UK ; New York, 2014).
[^7]: J. M. Lee, Introduction to Smooth Manifolds, 2nd ed (Springer, New York ; London, 2013).
[^8]: S. Carroll, Spacetime and Geometry: An Introduction to General Relativity, 3rd ed. (Pearson Education, 2013).
[^9]: S. Roman, Advanced Linear Algebra, 3rd ed (Springer, New York, 2007).
[^10]: A. Das, Tensors: The Mathematics of Relativity Theory and Continuum Mechanics (Springer, New York, 2007).
[^11]: E. Poisson, A Relativist’s Toolkit: The Mathematics of Black-Hole Mechanics (Cambridge University Press, Cambridge, UK ; New York, 2004).
[^12]: M. Spivak, A Comprehensive Introduction to Differential Geometry, 3rd ed (Publish or Perish, Inc, Houston, Tex, 1999).


