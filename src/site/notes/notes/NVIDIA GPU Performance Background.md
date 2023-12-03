---
{"dg-publish":true,"dg-path":"Blogs/NVIDIA GPU Performance Background.md","permalink":"/blogs/nvidia-gpu-performance-background/","dgPassFrontmatter":true,"noteIcon":"","created":"2023-08-04T19:01:06.000-06:00","updated":"2023-12-03T11:43:20.922-07:00"}
---

- Link: https://docs.nvidia.com/deeplearning/performance/dl-performance-gpu-background/index.html#understand-perf
### Take AWAYs 
##### Architecture 
> The GPU is a highly parallel processor architecture, composed of processing elements and a memory hierarchy. At a high level, NVIDIA® GPUs consist of a number of Streaming Multiprocessors (SMs), on-chip L2 cache, and high-bandwidth DRAM. Arithmetic and other ==instructions== are executed by the SMs; ==data and code are accessed from DRAM via the L2 cache.==

![Pasted image 20230703140230.png](/img/user/attachment/Pasted%20image%2020230703140230.png)

##### Tensor cores v.s. CUDA cors
> When math operations cannot be formulated in terms of matrix blocks they are executed in other CUDA cores. For example, the element-wise addition of two half-precision tensors would be performed by CUDA cores, rather than Tensor Cores.


### Performance
> Performance of a function on a given processor is limited by one of the following three factors: 
> - memory bandwidth, 
> - math bandwidth and 
> - latency.


#### Memory limited and math limited
>  $T_{mem}$ time is spent in accessing memory, $T_{math}$ time is spent performing math operations. If we further assume that memory and math portions of different threads can be ==overlapped==, the total time for the function is $\max(T_{mem}, T_{math})$ . The ==longer== of the two times demonstrates what limits performance: If math time is longer we say that a function is _math limited_, if memory time is longer then it is _memory limited_.

##### Memory time
> Memory time is equal to the number of bytes accessed in memory divided by the processor’s memory bandwidth.
> $$
> \#bytes/BW_{mem}
> $$

##### Math Time
> Math time is equal to the number of operations divided by the processor’s math bandwidth.
> > $$
> \#ops/BW_{math}
> $$

##### Arithmetic intensity
$$
\#ops/\#bytes
$$

##### `ops:byte` ratio
$$
BW_{math}/BW_{mem}
$$

> [!IMPORTANT]
> Thus, an algorithm is ==math limited== on a given processor if the algorithm’s arithmetic intensity is higher than the processor’s ops:byte ratio. Conversely, an algorithm is ==memory limited== if its arithmetic intensity is lower than the processor’s `ops:byte` ratio.


#### Latency 
> However, if the workload is ==not large enough==, or does not have sufficient parallelism, the processor will be under-utilized and performance will be limited by ==latency==.