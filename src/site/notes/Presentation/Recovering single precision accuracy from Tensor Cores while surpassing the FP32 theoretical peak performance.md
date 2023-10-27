---
{"dg-publish":true,"dg-path":"Blogs/Recovering single precision accuracy from Tensor Cores while surpassing the FP32 theoretical peak performance.md","permalink":"/blogs/recovering-single-precision-accuracy-from-tensor-cores-while-surpassing-the-fp-32-theoretical-peak-performance/","dgPassFrontmatter":true,"noteIcon":"","created":"2023-02-20T17:56:16.332-07:00","updated":"2023-10-27T14:36:41.146-06:00"}
---

#### Summary
This paper achieves the full fp32 precision recovering by improving Markidis‘s correction algorithm 

#### Pre knowledge
Tensor cores is to compute $$D = A\times B + C $$ Hence, ==before== going to tensor cores, A and B should be ==converted== to FP16 or TF32. The ==accumulator== will add C, which is a FP32 matrix. The final result $D$ is a FP32 matrix.   
 > [!NOTE]
 > The conversion loss is what Markidis‘s et al. consider mainly. 
#### Markidis‘s correction algorithm 

<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/paper-summary/recovering-single-precision-accuracy-from-tensor-cores-while-surpassing-the-fp-32-theoretical-peak-performance/#81411a" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



1. Algorithm ![Pasted image 20230208155355.png](/img/user/attachment/Pasted%20image%2020230208155355.png) 

</div></div>

#### Accurate?
###### Evaluation metric

<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/paper-summary/recovering-single-precision-accuracy-from-tensor-cores-while-surpassing-the-fp-32-theoretical-peak-performance/#0a8136" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



2. Accuracy Evaluation metric ![Pasted image 20230208155624.png](/img/user/attachment/Pasted%20image%2020230208155624.png) where $C_{FP64}$ is the reference matrix using FP64 to compute, i.e. $C_{FP64} = toFP64(A_{FP32})\cdot toFP64(B_{FP32})$ 

</div></div>

###### Accuracy comparison 
![Pasted image 20231027141349.png](/img/user/attachment/Pasted%20image%2020231027141349.png)
[[Paper Reading Annotate/Recovering single precision accuracy from Tensor Cores while surpassing the FP32 theoretical peak performance#^h0rsb5x90a\|Paper Reading Annotate/Recovering single precision accuracy from Tensor Cores while surpassing the FP32 theoretical peak performance#^h0rsb5x90a]]
#### Causes of not accurate
1. [[Paper Summary/Recovering single precision accuracy from Tensor Cores while surpassing the FP32 theoretical peak performance#Evaluate the mantissa loss\|Mantissa loss? ]]
2. [[Paper Summary/Recovering single precision accuracy from Tensor Cores while surpassing the FP32 theoretical peak performance#Evaluate the rounding for TC accumulator and improved\|Rounding mode of accumulator]]
3. [[Paper Summary/Recovering single precision accuracy from Tensor Cores while surpassing the FP32 theoretical peak performance#Evaluate the probability of underflow when computing $ Delta$\|Underflow when computing $\Delta$]]
#### The new algorithm

<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/paper-summary/recovering-single-precision-accuracy-from-tensor-cores-while-surpassing-the-fp-32-theoretical-peak-performance/#f0804b" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



2. Algorithm
	![Pasted image 20230209121606.png](/img/user/attachment/Pasted%20image%2020230209121606.png) 

</div></div>


#### Lessons

<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/paper-summary/recovering-single-precision-accuracy-from-tensor-cores-while-surpassing-the-fp-32-theoretical-peak-performance/#lessons" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



#### Lessons
1. Statistical analysis of floating-point
	- Expectation of mantissa bits
	- Probability of underflow
2. Power consumption is one important aspect need to be considered when using half precision algorithm. 
	> In recent year, the real machines of quantum computer have been developed and tried to be shown quantum supremacy, that they compute certain tasks that (classical) supercomputers are not be able to compute in realistic time. Moreover, since they have low power consumption [2], energy eﬃciency is becoming an important metric when evaluating quantum supremacy. For instance, qFlex is a quantum computer simulator based on tensor network contraction using single-precision complex matrix-matrix multiplication, where the power consumption of each component was reported during its simulation on Summit V100 GPUs [28]. Although they have considered to use FP16 and Tensor Cores in their simulation, ==they decided not to use it since FP16 has less exponent than FP32 and insuﬃcient to use.==


</div></div>


