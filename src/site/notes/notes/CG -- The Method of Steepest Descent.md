---
{"title":"CG -- The Method of Steepest Descent","tags":["Mathematical"],"dg-publish":true,"dg-path":"Blogs/CG -- The Method of Steepest Descent.md","permalink":"/blogs/cg-the-method-of-steepest-descent/","dgPassFrontmatter":true,"noteIcon":"","created":"2023-02-20T17:56:15.738-07:00","updated":"2023-10-24T01:26:33.473-06:00"}
---

### Definitions

![Pasted image 20221225215518.png](/img/user/attachment/Pasted image 20221225215518.png)
**Whenever you read "residual", think "direction of steepest descent"**

### Procedure

1. Starting from an initial point $x_0$
1. compute $r_0 = b-Ax_0$, which is the direction
1. Select next point $x_1 = x_0 = \alpha r_0$

#### How to decide $r_0$


Choose the vector which has the minimized increase of $f$, where the projection on the direction line ($r_0$) is 0

#### Final procedure
{ #b4988b}


![Pasted image 20221225220257.png](/img/user/attachment/Pasted image 20221225220257.png)

### Convergency

[CG -- Eigenvalues (Eigenvectors) and convergency](CG%20--%20Eigenvalues%20%28Eigenvectors%29%20and%20convergency.md)

#### Several Special cases

We are using the [Final procedure](CG%20--%20The%20Method%20of%20Steepest%20Descent.md#final-procedure).

##### Special Case 1

If $e\_{(i)}$ is an eigenvector with eigenvalue $\lambda\_{e}$
![Pasted image 20221228163248.png](/img/user/attachment/Pasted image 20221228163248.png)

 > [!INFO]
 > $r\_{(i)} = -Ae\_{(i)} = \lambda_ee\_{(i)}$ from [Definitions](CG%20--%20The%20Method%20of%20Steepest%20Descent.md#definitions) 

**==It takes only one step to converge to the exact solution==**

##### General Formula

If $e\_{(i)}$ is a linear combination of eigenvectors, and if $A$ is symmetric, there exists a set of $n$ orthogonal eigenvectors of $A$. We have the following:

Then


##### Special case 2

If $e_{(i)}$ has only one eigenvector component, then convergency is also achieved in ==one step== by choosing $\alpha_{(i)} = \lambda_{e}^{-1}.$

##### Special case 3

All the eigenvectors have a common eigenvalue $\lambda$


#### General Convergency

We have the formula [General Formula](CG%20--%20The%20Method%20of%20Steepest%20Descent.md#general-formula)

 > [!INFO]
 > We define energy norm to help
 > $$
 > ||e||_A = (e^TAe)^{1/2}
 > $$

##### Transfer minimizing $f(x)$ to a problem related to the energy norm

Recall for an arbitrary point $p$, and the solution point $x=A^{-1}b$, we have
$$
f(p)=f(x)+\frac{1}{2}(p-x)^{T}A(p-x)
$$
(From [Conjugate Gradient >
{ #6f2cce}
](Conjugate%20Gradient.md#6f2cce))

That is 
$$
f(x_{(i)})=f(x)+\frac{1}{2}e_{(i)}^{T}Ae_{(i)}
$$

Thus, ==minimizing $||e_{(i)}||_A$ is equivalent to minimizing $f(x_{(i)})$==

![500](/img/user/attachment/Pasted image 20221228165621.png)

 > [!INFO]
 > Recall $\xi$ and $\lambda$ in [General Formula](CG%20--%20The%20Method%20of%20Steepest%20Descent.md#general-formula)

##### Express $\omega$ as condition number slop (we defined)


 > [!INFO]
 > Here we only consider $n=2$
 > We define 
 > 
 > * the spectral condition number as $\kappa = \lambda_1/\lambda_2 > 1$,
 > * The slop of $e_{(i)}$ as $\mu = \xi_2/\xi_1$

![Pasted image 20221228170942.png](/img/user/attachment/Pasted image 20221228170942.png)

##### Plotting w.r.t. $\kappa$ and $\mu$

![Pasted image 20221228171025.png](/img/user/attachment/Pasted image 20221228171025.png)

The worst case is when $\mu = \pm\kappa$
That is, the larger its condition number$\kappa$), the slower the convergence of in Steepest Descent.
