---
{"dg-publish":true,"dg-path":"Blogs/Summarize NaN issues.md","permalink":"/blogs/summarize-na-n-issues/","dgPassFrontmatter":true,"noteIcon":"","created":"2024-03-06T18:18:26.979-07:00","updated":"2024-03-07T19:17:13.534-07:00"}
---

#### Define NaN
[[notes/IEEE - Special values (NaN, INF, subnormal)\|IEEE - Special values (NaN, INF, subnormal)]]
NaN is a floating-point representation to represent a value which has no meaning. Specifically, NaN's exponent is the maximum value for this datatype and mantissa is not 0. 
#### NaN's Generation
[[notes/How NaN generates\|How NaN generates]]
#### Example and analysis 
- [[notes/Which NaN, INF matters\|Which NaN, INF matters]]
- [[pytorch -- nan\|pytorch -- nan]]
	- More mixed precision issue related to NaN in [APEX](https://github.com/NVIDIA/apex/search?q=nan&type=issues)
- Search NaN in [deepstability](https://deepstability.github.io/)
- [sru](https://github.com/asappresearch/sru/issues/193)
- [cumf](https://github.com/cuMF/cumf_als.git)

##### Analyze NaN in matrix multiplication 
MM's implementation is like a blocked FMA, so the case producing NaN for FMA will also produce NaN in MM:
```
fma(+-0, +-inf, z) = NaN
fma(+-inf, +-0, z) = NaN
fma(x,y,-inf) = NaN if x*y=inf
fma(x,y,inf) = NaN if x*y=-inf
```
That's why it will produce NaN if you are doing precision conversion in the inputs; precision conversion is easily to get +-inf which will cause NaN in the following operations. 

