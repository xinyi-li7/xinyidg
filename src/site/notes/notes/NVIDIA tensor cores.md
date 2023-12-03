---
{"dg-publish":true,"dg-path":"Blogs/NVIDIA tensor cores.md","permalink":"/blogs/nvidia-tensor-cores/","dgPassFrontmatter":true,"noteIcon":"","created":"2023-07-25T17:38:17.000-06:00","updated":"2023-12-03T11:43:00.439-07:00"}
---

### Overview and evaluation of tensor cores
Tensor cores are firstly embedded in Volta architecture (CUDA 9), in Turing they are using the second generation, in Ampere the 3rd generation and 4th generation in Hopper

#### [Tensor Cores in Volta V100](https://images.nvidia.com/content/volta-architecture/pdf/volta-architecture-whitepaper.pdf)
###### Key take aways
> [!1]
> In Volta GV100, each Tensor Core performs 64 floating point FMA operations per clock, and eight Tensor Cores in an SM perform a total of 512 FMA operations (or 1024 individual floating point operations) per clock.

> [!2]
> Tensor Cores provide up to ==12x== higher peak TFLOPS on Tesla V100 that can be applied to deep learning training compared to using standard FP32 operations on P100

> [!3]
> Each Tensor Core operates on a ==4x4x4== matrix and performs the following operation: D = A×B + C, where A/B can be FP16 and C/D can be FP16/FP32

> [!4]
> ![Pasted image 20230630152326.png](/img/user/attachment/Pasted%20image%2020230630152326.png)
> Inputs are FP16; Full precision produce; FP32 accumulation. 

###### Manipulate methods 
1. CUDA C++ API. The API exposes specialized matrix load, matrix multiply and accumulate, and matrix store operations to efficiently use Tensor Cores from a CUDA-C++ program.
2. cuBLAS and cuDNN libraries.

#### [Tensor Cores in Turing](https://images.nvidia.com/aem-dam/en-zz/Solutions/design-visualization/technologies/turing-architecture/NVIDIA-Turing-Architecture-Whitepaper.pdf)
###### Key take aways
1. Add new ==INT8 and INT4== for inferencing. 
2. A new technique called Deep Learning Super Sampling (DLSS) is powered by Tensor Cores
#### Tensor Cores in Ampere 
###### [GA10X Key take aways](https://www.nvidia.com/content/PDF/nvidia-ampere-ga-102-gpu-architecture-whitepaper-v2.pdf)
1. Introduce hardware support for processing matrices with specific ==sparsity== patterns at up to 2x throughput, by skipping the zero-valued elements.
2. Add new precision mode ==TF32 and BF16==.
3. Note that GA10x GPUs do not include Tensor Core acceleration for double-precision (FP64) operations, as provided in A100.
###### [A100 Key take aways](https://images.nvidia.com/aem-dam/en-zz/Solutions/data-center/nvidia-ampere-architecture-whitepaper.pdf)
>[!1] 
>For HPC, the A100 Tensor Core includes new IEEE-compliant FP64 processing that delivers 2.5x the FP64 performance of V100.
>-  ==TF32== Tensor Core instructions which accelerate processing of FP32 data
> - IEEE-compliant ==FP64== Tensor Core instructions for HPC
> - ==BF16== Tensor Core instructions at the same throughput as FP16

> [!2]
> Each of the A100 Tensor Cores can execute 256 FP16 FMA operations per clock, allowing it to compute the results for an ==8x4x8== mixed-precision matrix multiplication per clock

> [!3]
> A100 accelerates tensor math with TF32 while supporting ==FP32 input and output== data (right), enabling easy integration into DL and HPC programs and automatic acceleration of DL frameworks.

> *FP64 facility*
> - For ==HPC== community. 
> - Delivering up to ==2.5x== the FP64 performance of the NVIDIA Tesla V100 GPU
> - The new Double Precision Matrix Multiply Add instruction on A100 replaces 8 DFMA instructions on V100, reducing instruction fetches, scheduling overhead, register reads, datapath power, and shared memory read bandwidth.
> - The Tensor Core Accelerated ==Iterative Refinement Solve==r (TCAIRS) in cuSOLVER automates usage of mixed precision for this application.
> 	-  Last year, a fusion reaction study for the International Thermonuclear Experimental Reactor demonstrated that mixed-precision techniques delivered a speedup of 3.5x on V100 for such solvers using V100’s FP16 Tensor Cores. The same technology used in that study tripled the ==Summit== supercomputer’s performance on the HPL-AI benchmark.
### Comparison of these three generations

> [!IMPORTANT]
> 
<div class="transclusion internal-embed is-loaded"><div class="markdown-embed">

$<div class="markdown-embed-title">

# Dissecting Tensor Cores via Microbenchmarks- Latency, Throughput and Numerical Behaviors

</div>


2. Furthermore, Ampere Architecture redesigns the micro-architecture of Tensor Cores. Unlike Volta and Turing Architecture ==which have eight Tensor Cores per SM and each Tensor Core performs a 4×4×4 MM (i.e. m = n = k = 4), there are only four Tensor Cores per SM and each Tensor Core performs an 8×4×8 MM==. 

</div></div>

#### 
![Pasted image 20230703173135.png](/img/user/attachment/Pasted%20image%2020230703173135.png)
\- From [[Dissecting the NVidia Turing T4 GPU via Microbenchmarking.pdf]]
### Ways to manipulate tensor cores
1. High-level libraries like cuBLAS and cuDNN
2. CUDA C++ API (WMMA)
3. PTX
4. Device 

Example of 1, 2 in [Programming Tensor Cores in CUDA 9](https://developer.nvidia.com/blog/programming-tensor-cores-cuda-9/)

Example of 3, 4 in [[NVIDIA cutlass, PTX to program tensor cores.pdf]]

### Use CUDA C++ API (NVIDIA:wmma)
[[notes/Programming tensor cores using nvcuda-wmma\|Programming tensor cores using nvcuda-wmma]]
