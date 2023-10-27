---
{"dg-publish":true,"permalink":"/paper-summary/the-effects-of-numerical-precision-in-scientific-applications/","dgPassFrontmatter":true,"noteIcon":""}
---


[[THE EFFECTS OF NUMERICAL PRECISION IN SCIENTIFIC APPLICATIONS.pdf]]
#### Take aways
1. > Since neither posit nor 16-bit floating-point formats are supported in commercial general-purpose processors
	
	Can be one motivation? NVIDIA GPUs seems to be the very few available hardware for half precision.
2. How to evaluate: 
	- use three HPC applications and three ML apps
	- Use 64-bit results as ground truth
	- Run those apps with specific formats
	- Compute the MSE with ground truth 