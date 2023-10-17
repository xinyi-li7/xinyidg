---
{"dg-publish":true,"dg-home":true,"permalink":"/paper-summary/a-survey-of-numerical-methods-utilizing-mixed-precision-arithmetic/","tags":["gardenEntry"],"dgPassFrontmatter":true}
---





[[A Survey of Numerical Methods Utilizing Mixed.pdf]]
## Introduction 
- [ ] What is memory bandwidth
###### Take away
- Data access and communication is more expensive compared to arithmetic operations. 
- A good motivation:
	> the US Exascale Computing Project decided for the aggressive step of building a multiprecision focus effort to take on the challenge of designing and engineering novel algorithms exploiting the compute power available in low precision and adjusting the communication format to the application specific needs
###### Need to answer after finishing
- [ ] List the technologies that may not yet be considered mature
## Dense Linear Algebra
### Performance data from different manufacture
> Half precision also enables machine learning applications to run faster, not only because of the faster arithmetic, ==but also because of the reduction in memory storage and traffic by a factor ==of ==2×== against single precision, and by a factor of ==4×== against double precision.

> AMD also provides half-precision capabilities, and their software stack shows support for both the bfloat16 format and the IEEE format [2]. The theoretical performance of half-precision on AMD GPUs follows the natural 2×/4× speedups against single/double precisions, respectively.

> NVIDIA GPUs implement the “binary16” format which is defined by the IEEE-754 standard [2]. While the Pascal GPU architecture introduced hardware support for FP16 arithmetic, the Volta architecture, which powers the Summit supercomputer,1 comes with hardware acceleration units (called Tensor Cores) for matrix multiplication in FP16. 
> 
> These Tensor Cores are theoretically ==12×== faster than the theoretical FP16 peak performance of the preceding architecture. 
> Applications taking advantage of the Tensor Cores can run up to ==4×== faster than using the regular FP16 arithmetic on the same GPU. The Tensor Cores are also able to perform a mixe precision multiplication, with a low precision input (e.g. half-precision) and a higher precision output (typically single-precision).
### Hardware
#### Tensor cores
- Motivation: 
	> Apart from the vendor library, taking advantage of the Tensor Cores in a custom kernel is possible also through the use of low-level APIs that are provided by the programming model.
	![Pasted image 20230217164816.png](/img/user/attachment/Pasted%20image%2020230217164816.png)

    > However, using the explicit instruction may lead to long-term compatibility issues for open-source libraries as new architectures are released.
    
#### HGEMM
###### Three types
- HGEMM-FP16 ==output== (tensor cores ==on==)
- HGEMM-FP32 ==output== (tensor cores ==on==)
- HGEMM-FP32 ==output== (tensor cores ==on==)
###### Performance v.s. accuracy (forward error)
> Performing HGEMM with FP32 output achieves at least ==two== more digits of accuracy when compared with the other two HGEMM variants.
#### Open source HGEMM
- [ ] MAGMA library [5][6]
- Performance: MAGMA outperforms than cuBLAS for small size matrix. 
### Iterative refinement: recover precision 
#### Classic
- Used when the iterative correction is cheaper than the original computation. 
- In the high overhead computation (e.g. LU decomposition $O(n^3)$), use low precision and then recover the precision by iterative refinement (iterative parts will use high precision).
- Three tasks
	- Original solve/factorization
	- Residual computation
	- Correction equation solve
- [14] developed a three-precision iterative refinement scheme. 
- [ ] Check CEN... method
- [ ] Find some applications using this method
- [ ] Forward error v.s. backward error
#### GMRES-IR
- Use GMRES as the solver for ==correction equation==
- the condition number can be relaxed to $<10^8$ 
- Compare
	- classic ![Pasted image 20230223000620.png](/img/user/attachment/Pasted%20image%2020230223000620.png)
	- GMRES ![Pasted image 20230223000633.png](/img/user/attachment/Pasted%20image%2020230223000633.png)

#### Scaling 
- Can use two-sided diagonal scaling to $A$, $A$ is replaced by $RAS$
	![Pasted image 20230223001040.png](/img/user/attachment/Pasted%20image%2020230223001040.png)
- This is for general and symmetric matrix
#### Mixed-precision Factorization
- [ ] Haidar at al. [3] proposed IR methods using mixed-precision factorizations. (Apply higher precision  at critical parts)
- FP16 can be used to recover FP63 accuracy
- Motivation of mixed precision factorization: Get extra precision when working with very low precisions, so we can converge faster
- One building block: mixed precision BLAS
- Mixed precision iterative refinement is 4X more faster than FP64 achieving in the same accuracy due to some optimizations:
	1. Fusing al data conversions with computational kernels
	2. Use mixed precision factorization. 
### Cholesky Factorization
- Challenge for FP16 since the matrix is symmetric positive definite
	1. If fp16 is used then the limited range might cause overﬂow during the rounding -- [[Paper Summary/A survey of Numerical Methods utilizing mixed precision arithmetic#\|#]]
	2. The converted matrix may not be positive definite in FP16 [[Paper Summary/A survey of Numerical Methods utilizing mixed precision arithmetic#Scaling for Cholesky Factorization\|#Scaling for Cholesky Factorization]]
#### Scaling for Cholesky Factorization

#### shifting 
###
> [!Take Away]
> **From [[Paper Summary/A survey of Numerical Methods utilizing mixed precision arithmetic#HGEMM\|#HGEMM]]**
> Given that HGEMM with FP32 output is mostly within 90% of the peak tensor core throughput, it is clearly the best option for mixed-precision algorithms that target achieving higher accuracy while taking advantage of the half-precision.
> 
> **From [[Paper Summary/A survey of Numerical Methods utilizing mixed precision arithmetic#Mixed-precision Factorization\|#Mixed-precision Factorization]]**
> The developments were applied to GPU Tensor Cores and illustrate that FP16 can be used to get FP64 accuracy for problems with κ ∞ (A) of up to 10 5 , compared to a more typical requirement of κ ∞(A) < 10 4.

### Mixed precision sparse factorizations
###### High level idea
- Sparse systems are using ==direct methods==
- Perform expensive computation in low precision, then ==fixup== in higher precision
###### Steps of sparse (LU/QR) factorizations 
1. Gather the values from sparse data structures into contiguous memory
2. Perform GEMM operations 
3. Scatter the output of GEMM into destination sparse data structure
###### Motivation to use lower precision
- Use lower precision GEMM function in step 2 (limited improvement)
- Use lower precision to reduce the data movement in step 1 and 3
###### Use IR to recover the precision when computing in low precision
- The iterative refinement with three precision 
- Have proved the error bounds in [49] regarding the condition number. 
> [!INFO]
> The IR algorithm to recover precision for low precision sparse matrix is available in xGERFSX functions in LAPACK.

###### Open questions
- • When $\epsilon_w$ is bﬂoat16, the error analysis and error bounds may need be revisited.
- Thus, the ratio between ”expensive” and ”cheap” is smaller than the dense case. We need to be more mindful with the higher precision calculations.
# Thoughts
###### From Scaling
> Despite the large literature on scaling such problems, no clear conclusions are available on when or how one should scale; see [21] for a recent experimental study.

###### Most of method will use high precision when computing residuals
# Motivation
Kasia's paper: people struggle to find a good solver
