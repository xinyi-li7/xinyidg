---
{"title":"CG -- Eigenvalues (Eigenvectors) and convergency","tags":["Mathematical"],"dg-publish":true,"permalink":"/cg/cg-eigenvalues-eigenvectors-and-convergency/","dgPassFrontmatter":true}
---

### Apply a matrix to Eigenvectors \<=> Apply an eigenvalue to this vector


Iterative methods often depend on applying matrix $B$ to a vector over and over again. 

 > 
 > When $B$ is repeatedly applied to an eigenvector $v$, one of two things can happen. 
 > 
 > * If $|\lambda<1|$, then $B^iv = \lambda^iv$ will vanish as $i$ approaches infinity (Figure 9).
 > * If $|\lambda| > 1$, then $B^iv$ will grow to infinity (Figure 10). 

-cite from [An Introduction to the Conjugate Gradient Method Without the Agonizing Pain](../../../Paper%20Reading%20Annotate/An%20Introduction%20to%20the%20Conjugate%20Gradient%20Method%20Without%20the%20Agonizing%20Pain.md)

### Any vector can be written as the sum of eigenvectors if matrix $B$ is symmetric

If $B$ is ==symmetric== (and often if it is not), then there exists a set of $n$ ==linearly independent==eigenvectors of $B$ denoted $v_1, v_2, ...., v_n$. 

Thus, one can examine the effect of $B$ on each eigenvector separately.

### Example: Jacobi iterations

The matrix $A$ is split into two parts: 

* $D$, whose diagonal elements are identical to those of $A$, and whose off-diagonal elements are zero; 
* $E$ , whose whose diagonal elements are zero, and whose off-diagonal elements are identical to those of $A$. 
  That is 
  $$
  A = D + E
  $$
  
  

Suppose we start with some arbitrary vector $x\_{(0)}$. For each iteration, we ==apply $B$== to this vector, then add $z$ to the result. 


###### Spectral radius of a matrix $\rho$

$\rho(B) = max|\lambda_i|$, $\lambda_i$ is an eigenvalue of $B$

##### 

Thus, if $\rho(B)\<1$, then the error term $e\_{(i)}$ will converge to zero as $i$ approaches infinity. Hence, we have a ==guarantee of convergency ==regardless of the initial vector $x\_{(0)}$ . 
