---
{"dg-publish":true,"dg-path":"Blogs/How NaN generates.md","permalink":"/blogs/how-na-n-generates/","dgPassFrontmatter":true,"noteIcon":"","created":"2023-02-20T17:56:12.196-07:00","updated":"2024-03-07T19:20:41.462-07:00"}
---

Basic arithmetic
![Pasted image 20221223121615.png](/img/user/attachment/Pasted%20image%2020221223121615.png)
![Pasted image 20240307182940.png](/img/user/attachment/Pasted%20image%2020240307182940.png)
\-cite: [[Paper Reading Annotate/Numerical Computation Guide\|Numerical Computation Guide]]

### Math operation (part of cuda math)
##### Input data are not in the range
acos, acosh,asin, atanh, erfcinv, log2/10/ , sqrt, normcdfinvf

##### special inputs -> nan
cos(+-inf)
cospi(+-inf)
fma(0*inf, or inf-inf)
fmod(+-inf, y), fmodf(x, 0)
jn(n, x) = NaN if n<0
pow(x, y) if y is not integer
remainder(x, +-0) = NaN
remainder(inf, y) = NaN
##### Propagate NaN
fabs(NaN)
max/min(NaN, NaN)
fmod(NaN, y) or fmod(x, NaN)'
j0/j1/jn(NaN) = NaN
nextafterf(x, y) = NaN if either x or y is NaN
##### Not propagate NaN
max/min(x, NaN) where x is not NaN
hypotf(+-inf, NaN) = inf
ilogb(NaN) = INT_MIN
norm3d, norm4d, norm if either of the parameters is inf, it will return inf even if there exists NaN
pow(1, NaN) = 1, pow(NaN, +-0) = 1
