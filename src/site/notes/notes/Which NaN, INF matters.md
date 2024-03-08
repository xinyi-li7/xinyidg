---
{"dg-publish":true,"dg-path":"Blogs/Which NaN, INF matters.md","permalink":"/blogs/which-na-n-inf-matters/","dgPassFrontmatter":true,"noteIcon":"","created":"2023-02-20T17:56:16.166-07:00","updated":"2024-03-07T19:00:47.333-07:00"}
---

1. NaN, INF in the output -- NaN, INF is propagated through the codes, need a tool to help detect how these NaN, INF
2. NaN, INF are not in the outputs:
	1. Eliminating them in a proper way
		1. Throw them (automatic mixed precision)
		2. max not propagate them 
		3. In matrix operations
	2. Incorrectly not propagate them

