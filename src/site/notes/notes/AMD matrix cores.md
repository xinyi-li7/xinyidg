---
{"dg-publish":true,"dg-path":"Blogs/AMD matrix cores.md","permalink":"/blogs/amd-matrix-cores/","dgPassFrontmatter":true,"noteIcon":"","created":"2023-07-27T16:09:19.000-06:00","updated":"2023-12-03T11:44:36.654-07:00"}
---

### [AMD matrix cores](https://gpuopen.com/learn/amd-lab-notes/amd-lab-notes-matrix-cores-readme/)
#### Manipulate the matrix cores
##### Summary
To manipulate the matrix cores, you can
1. At high-level way, just use libraries such as rocBLAS or ==rocWMMA==(similar to nvidia:wmma+) 
2. write GPU kernels entirely in assembly (which can be somewhat challenging and impractical)
3. sprinkle HIP kernels with inline assembly
4. ==Use compiler intrinsics==: these represent the assembly instructions in such a way that the compiler knows about the semantics and requirements
AMD detailed how to use ==compiler intrinsics==. 

#### [Difference between using rocWMMA and compiler intrinsics](https://github.com/ROCmSoftwarePlatform/rocWMMA/issues/202)


#### Using compiler intrinsics
>[!Note]
> Note that the AMD's warp(namely wavefront in their [official doc](https://www.olcf.ornl.gov/wp-content/uploads/2019/10/ORNL_Application_Readiness_Workshop-AMD_GPU_Basics.pdf)) is 64 instead of 32 in CUDA
#### Core function
``` 
d= __builtin_amdgcn_mfma_CDFmt_MxNxKABFmt (a, b, c, cbsz, abid, blgp)
```

#### How work in distributed 
Work were distributed among warps. Specifically, each `mfma` compiler intrinsics have been decided which thread store which locations of `A`, `B`, `D`. In their [labnotes](https://gpuopen.com/learn/amd-lab-notes/amd-lab-notes-matrix-cores-readme/), they give some examples. 

They didn't define the arithmetic operations. We guess these operations are performed with the matrix cores hardware.
[[Experiments for order\|Experiments for order]]
[[notes/Programming tensor cores using nvcuda-wmma#How work is distributed among the warp\|mirror in NVIDIA GPU]]
#### High-level idea
Each core function have a fixed layout. Check it using https://github.com/RadeonOpenCompute/amd_matrix_instruction_calculator

#### Questions
1. Matrix cores speedup is 2x compared to simd throughput, which is much slower than NVIDIA's tensor cores? 