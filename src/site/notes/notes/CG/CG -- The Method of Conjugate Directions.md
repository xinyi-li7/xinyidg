---
{"title":"CG -- The Method of Conjugate Directions","tags":["Mathematical"],"dg-publish":true,"permalink":"/notes/cg/cg-the-method-of-conjugate-directions/","dgPassFrontmatter":true}
---

### High-level idea
[[notes/CG/CG -- The Method of Steepest Descent\|CG -- The Method of Steepest Descent]] often finds itself taking steps in the same direction as earlier steps. So we have an idea to make it converge faster: 
> Let’s pick a set of orthogonal search directions $d_{(0)}, d_{(1)}, ..., d_{(n-1)}$. In each search direction, we'll take exactly one step and that step will be just the right length to line up with $x$. After $n$ steps, we'l be done. 

> [!example]
> ![Pasted image 20221229225619.png](/img/user/attachment/Pasted%20image%2020221229225619.png)

### Update procedure
$$
x_{(i+1)} = x_{(i)} + \alpha_{(i)}d_{(i)}
$$
#### Compute \alpha
In order not to step in the previous directions ($d_{(i)}$) again, $e_{(i+1)}$ should be ==orthogonal to== $d_{(i)}$. So, 
![Pasted image 20230102151637.png](/img/user/attachment/Pasted%20image%2020230102151637.png)

But we cannot do anything without knowing $e_{(i)}$. But if we know $e_{(i)}$, we can solve the problem. 

### A-orthogonal instead of orthogonal
> [!Definition]
> Two vectors $d_{(i)}$ and $d_{(j)}$ are A-orthogonal, or conjugate if 
> $$
> d^T_{(i)}Ad_{(j)}=0
> $$
> ![Pasted image 20230102152835.png](/img/user/attachment/Pasted%20image%2020230102152835.png)

Thus, $\alpha$ becomes from [[notes/CG/CG -- The Method of Conjugate Directions#Compute alpha\|#Compute alpha]] to
![Pasted image 20230102153408.png](/img/user/attachment/Pasted%20image%2020230102153408.png)

##### Prove we can compute $x$ in $n$ steps
![Pasted image 20230102154006.png](/img/user/attachment/Pasted%20image%2020230102154006.png)
From the above formula, we concludes that ==$\alpha_{(i)}=-\delta_{(i)}$==
![Pasted image 20230102154720.png](/img/user/attachment/Pasted%20image%2020230102154720.png)
### Construct ${d_{(i)}}$ using Gram-Schmidt Conjugation
[[notes/CG/CG -- Gram-Schmidt Conjugation\|CG -- Gram-Schmidt Conjugation]]
### Properties if using the Method of Conjugate Directions
1. The error term is evermore A-orthogonal to all the old search directions since we never step back in the previous directions (Also from equation 35).
2. From 1, since $r_{(i)}=-Ae_{(i)}$, the residual is evermore orthogonal to all the old search directions, that is ![Pasted image 20230102165215.png](/img/user/attachment/Pasted%20image%2020230102165215.png)
3. From 2, because the search directions ({$d_{(i)}$}) are constructed from the $u$ vectors, the residual $r_{(i)}$ is orthogonal to these previous $u$ vectors, that is ![Pasted image 20230102165302.png](/img/user/attachment/Pasted%20image%2020230102165302.png)
4. From 2 and 3, we have ![Pasted image 20230102165323.png](/img/user/attachment/Pasted%20image%2020230102165323.png)

> [!INFO]
> ![Pasted image 20230102165357.png](/img/user/attachment/Pasted%20image%2020230102165357.png)

### New updates formula
![Pasted image 20230102165444.png](/img/user/attachment/Pasted%20image%2020230102165444.png)