---
{"title":"Conjugate Gradient","tags":["Mathematical"],"dg-publish":true,"permalink":"/notes/conjugate-gradient/","dgPassFrontmatter":true,"noteIcon":""}
---

### Motivation

Solve large systems of linear equations
$$
Ax = b
$$
where $x$ is an unknown vector, $b$ is a known vector, and $A$ is a known, square, symmetric, positive-deﬁnite (or positive-indeﬁnite) matrix.

### Why symmetric and Why positive

In short, if $A$ is symmetric and positive-deﬁnite, a quadratic function is minimized by the solution to $Ax = b$ .

##### ==This linear system problem can be transfer to an optimization problem==

1. We have a quadratic function of vector $x$
   $$
   f(x) = \frac{1}{2}x^TAx - b^Tx +c
   $$
   
{ #6f2cce}

1. Gradient of $f'(x)$ is
   $$
   f'(x) = \frac{1}{2}A^Tx + \frac{1}{2}Ax - b
   $$
1. If $A$ is symmetric
   $$
   f'(x) = Ax -b
   $$
   Therefore, $Ax=b$ is critical point of $f(x)$. ($f'(x) = 0$)
1. If $A$ is positive-define, that is for every nonzero vector $x$, $x^TAx > 0$, $f(x)$ is an increasing function and $Ax=b$ can be solved by finding an $x$ that minimizes $f(x)$.

  
 > [!NOTE]
 > Different Situations w.r.t. positive or not
 > ![Pasted image 20221225214120.png](/img/user/attachment/Pasted image 20221225214120.png)

### [CG -- The Method of Steepest Descent](CG%20--%20The%20Method%20of%20Steepest%20Descent.md)

<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/notes/cg-the-method-of-steepest-descent/#final-procedure" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



#### Final procedure


![Pasted image 20221225220257.png](/img/user/attachment/Pasted image 20221225220257.png)


</div></div>


### [CG -- The Method of Conjugate Directions](CG%20--%20The%20Method%20of%20Conjugate%20Directions.md)
### The Method of Conjugate Gradients
#### High level idea
In [[notes/CG -- The Method of Conjugate Directions\|CG -- The Method of Conjugate Directions]] we can set $u_i = r_i$.
#### Reason
![Pasted image 20230102165650.png](/img/user/attachment/Pasted%20image%2020230102165650.png)
![Pasted image 20230102165728.png](/img/user/attachment/Pasted%20image%2020230102165728.png)
![Pasted image 20230102170903.png](/img/user/attachment/Pasted%20image%2020230102170903.png)
#### Compute \beta
![Pasted image 20230102170928.png](/img/user/attachment/Pasted%20image%2020230102170928.png)
We don't need to memory all previous vectors like [[notes/CG -- Gram-Schmidt Conjugation#Difficulties\|CG -- Gram-Schmidt Conjugation#Difficulties]]. 
### CG procedure
![Pasted image 20230102171046.png](/img/user/attachment/Pasted%20image%2020230102171046.png)

[HPCG (Understand the preconditioned conjugate gradient method )](HPCG%20(Understand%20the%20preconditioned%20conjugate%20gradient%20method%20).md)


