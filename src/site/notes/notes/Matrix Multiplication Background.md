---
{"dg-publish":true,"dg-path":"Blogs/Matrix Multiplication Background.md","permalink":"/blogs/matrix-multiplication-background/","dgPassFrontmatter":true,"noteIcon":"","created":"2023-07-25T17:37:41.000-06:00","updated":"2023-12-03T11:43:10.489-07:00"}
---

- link: https://docs.nvidia.com/deeplearning/performance/dl-performance-matrix-multiplication/index.html

Can read [[notes/NVIDIA GPU Performance Background\|NVIDIA GPU Performance Background]] firstly
### Overview

#### Problem formulation
GEMM is defined as the operation 
$$
C = \alpha AB+ \beta C
$$

#### Terminology
$A$ is an $M \times K$ matrix
$B$ is a $K\times N$ matrix 
$C$ is an $M\times N$ matrix.

### Math and Memory Bounds

#### Computations
$M*N*K$ FMAs are needed to do this matrix multiplication. Since each FMA is 2 operations,  a total $2*M*N*K$ FLOS are required. 

#### Math limit? or Memory Limit? 
Defs are from [[notes/NVIDIA GPU Performance Background\|NVIDIA GPU Performance Background]]
##### Arithmetic Intensity
Arithmetic Intensity = 
$$
\frac{\# of FLOPS}{\# of byte accesses} = \frac{2\cdot (M\cdot  N\cdot K)}{2\cdot (M\cdot K) + (N\cdot K) + (M\cdot N)} 
= \frac{M\cdot N \cdot K}{M\cdot K + N\cdot K + M\cdot N}
$$

### GPU implementation of GEMM
> GPUs implement GEMMs by partitioning the output matrix into ==tiles==, which are then assigned to thread blocks.

> Each thread block computes its output tile by ==stepping through the K dimension== in tiles, loading the required values from the A and B matrices, and multiplying and accumulating them into the output.
> ![Pasted image 20230703143900.png](/img/user/attachment/Pasted%20image%2020230703143900.png)

#### Better Performance using tensor cores
> Tensor Cores depend on NVIDIA library versions. Performance is better when equivalent matrix dimensions M, N, and K are aligned to== multiples of 16 bytes== (or 128 bytes on A100). With NVIDIA cuBLAS versions before 11.0 or NVIDIA cuDNN versions before 7.6.3, this is a requirement to use Tensor Cores;

#### Decide the tiling size
> While multiple tiling strategies are available,==larger tiles have more data reuse, allowing them to use less bandwidth and be more efficient than smaller tiles==. On the other hand, for a problem of a given size, using larger tiles will generate fewer tiles to run in parallel, which can potentially lead to ==under-utilization== of the GPU.

[[notes/Tiled Matrix Multiplication -- CUDA implementation\|Tiled Matrix Multiplication -- CUDA implementation]]