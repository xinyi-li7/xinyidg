---
{"dg-publish":true,"permalink":"/pytorch-nan/","dgPassFrontmatter":true,"noteIcon":"","created":"2024-03-07T19:00:57.604-07:00","updated":"2024-03-07T19:04:46.401-07:00"}
---

- link: https://github.com/pytorch/pytorch/issues/40497
##### Issue: NaN loss
##### Guessing
Using mixed precision will cause non-scaled gradient became NaN which caused the final loss as NaN 