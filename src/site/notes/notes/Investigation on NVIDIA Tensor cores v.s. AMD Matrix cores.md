---
{"dg-publish":true,"dg-path":"Blogs/Investigation on NVIDIA Tensor cores v.s. AMD Matrix cores.md","permalink":"/blogs/investigation-on-nvidia-tensor-cores-v-s-amd-matrix-cores/","dgPassFrontmatter":true,"noteIcon":"","created":"2023-07-13T15:24:05.000-06:00","updated":"2023-12-03T11:46:18.035-07:00"}
---

### Questions want to solve
1. What are formats for matrix multiplication in tensor cores and Matrix cores?
2. Is there a way or algorithm which can translate `wmma` in CUDA to Matrix cores operation? 
3. How to design the experiments to test the numerical computation ability of tensor cores and matrix cores? 
### Related doc
[[notes/AMD matrix cores\|notes/AMD matrix cores]]
[[notes/NVIDIA tensor cores\|notes/NVIDIA tensor cores]]
[[notes/Matrix Multiplication Background\|Matrix Multiplication Background]]
### Manipulate Comparison
| Methodology                     | Vender | Description | Mapping Vender Methodology |
| ------------------------------- | ------ | ----------- | -------------------------- |
| [[notes/Warp Matrix Functions\|notes/Warp Matrix Functions]] | NVIDIA |             | rocWMMA                    |
| rocWMMA                         | AMD    |             | Warp Matrix Functions      |
| PTX wmma/mma instruction        | NVIDIA |             | MFMA compiler intrinsic    |
| MFMA compiler intrinsic         | AMD    |             | PTX wmma/mma instruction   |
| cuBLAS                          | NVIDIA |             | rocBLAS                    |
| rocBLAS                         | AMD    |             | cuBLAS                     |
|                                 |        |             |                            |
#### Supported formats
|                     | Inputs | outputs |
| ------------------- | ------ | ------- |
| NVIDIA Tensor cores |  fp16/bf16/tf32/fp64      |  fp32/fp16/bf16/fp64       |
| AMD Matrix Cores    |  fp16/bf16/fp32/fp64      |   fp32/fp16/bf16/fp64      |


Level 2 or 3 
UTK test cases, mixed precision testing on GPUs
#### Manipulate Method