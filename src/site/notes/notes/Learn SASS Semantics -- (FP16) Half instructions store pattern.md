---
{"title":"SASS Semantics -- Half instructions store pattern","tags":["CUDA","SASS"],"date":"2022-05-12","dg-publish":true,"dg-path":"Blogs/Learn SASS Semantics -- (FP16) Half instructions store pattern.md","permalink":"/blogs/learn-sass-semantics-fp-16-half-instructions-store-pattern/","dgPassFrontmatter":true,"noteIcon":""}
---


#### Store Pattern in registers
[[Paper Reading Annotate/Exploiting half precision arithmetic in Nvidia GPUs#^zerj3islv8i\|Exploiting half precision arithmetic in Nvidia GPUs#^zerj3islv8i]]
#### TODO

* [ ] For half 
  
  * [ ] 
* [ ] For half2
  
  * [ ] Learn [CUDA -- half2](CUDA%20--%20half2.md)
* [ ] Try on real code -- cutlass


#### Pattern summary from [others](https://forums.developer.nvidia.com/t/nvidia-pascal-titan-xp-titan-x-geforce-gtx-1080-ti-gtx-1080-gtx-1070-gtx-1060-gtx-1050-gt-1030/42660/113) (not necessarily correct)

````
HFMA2.<MRG_H0|MRG_H1|F32>.<FTZ|FMZ>.<SAT> d, a.<H0_H0|H1_H1|F32>, <->b.<H0_H0|H1_H1>, <->c.<H0_H0|H1_H1|F32>; 
````

````
<HADD2|HMUL2>.<MRG_H0|MRG_H1|F32>.<FTZ|FMZ>.<SAT> d, a.<H0_H0|H1_H1|F32>, <->b.<H0_H0|H1_H1>;
````

#### Result

|opcodes|\# of operands|operand type|Destination|store pattern|comments|
|-------|-------------|------------|-----------|-------------|--------|
|`HADD2.FP32`|3 operands|REG;IMM_UINT64;CBANK|First operands; REG|[Learn SASS Semantics -- (FP16) Half instructions store pattern](Learn%20SASS%20Semantics%20--%20(FP16)%20Half%20instructions%20store%20pattern.md)|Seems it transfer FP16 to FP32 by adding 0|
|`HADD2`|4 operands|REG;IMM_UINT64;CBANK|First two operands seems to store the same result; REG|[Learn SASS Semantics -- (FP16) Half instructions store pattern](Learn%20SASS%20Semantics%20--%20(FP16)%20Half%20instructions%20store%20pattern.md)|See issue [Lower 16-bits are zero in HADD2 four operands case](../../../notes/Learn%20SASS%20Semantics%20--%20FP16.md#lower-16-bits-are-zero-in-hadd2-four-operands-case)|
|`HADD2`|3 operands|REG;IMM_UINT64;CBANK|First operands; REG|[Learn SASS Semantics -- (FP16) Half instructions store pattern](Learn%20SASS%20Semantics%20--%20(FP16)%20Half%20instructions%20store%20pattern.md)||
|`HADD2.FP32`|4 operands|?|?|[Learn SASS Semantics -- (FP16) Half instructions store pattern](Learn%20SASS%20Semantics%20--%20(FP16)%20Half%20instructions%20store%20pattern.md)||
||||||||

#### Exploration

[Learn SASS Semantics FP16 -- H0_H0 or H1_H1](Learn%20SASS%20Semantics%20FP16%20--%20H0_H0%20or%20H1_H1.md)
[Learn SASS Semantics -- (FP16) Half instructions store pattern](Learn%20SASS%20Semantics%20--%20(FP16)%20Half%20instructions%20store%20pattern.md) 

#### Issues

##### Lower 16-bits are zero in `HADD2` four operands case

`uint16_t` takes the lower 16 bits of other format.See https://stackoverflow.com/questions/53882934/extract-upper-and-lower-word-of-an-unsigned-32-bit-integer

#### Resource

https://forums.developer.nvidia.com/t/nvidia-pascal-titan-xp-titan-x-geforce-gtx-1080-ti-gtx-1080-gtx-1070-gtx-1060-gtx-1050-gt-1030/42660/113

### ==\<HFMA2|HADD2|HMUL2>.FP32==

Before executing this instruction, the stored datatype is FP16;
After executing this instruction, the stored datatype is FP32.

### ==R#.\<H0_H0|H1_H1>==

From [Learn SASS Semantics FP16 -- H0_H0 or H1_H1](Learn%20SASS%20Semantics%20FP16%20--%20H0_H0%20or%20H1_H1.md)
#### Conclusion

`H0_H0` means lower 16bits of the 32-bit register, we can just use 

````cpp
uint16_t val = R4_value
````

to extract the value;

`H1_H1` means lower 16bits of the 32-bit register, we can just use 

````cpp
uint16_t val = R4_value >> 16
````

to extract the value;

### ==\<HADD2|HMUL2> with 4 operands==

From [Write and analyze a FP16 CUDA program > Use half2 and perform addition using half2 arithmetic functions](Write%20and%20analyze%20a%20FP16%20CUDA%20program.md#use-half2-and-perform-addition-using-half2-arithmetic-functions), it seems it will appear when we have two constants as the direct arguments for `half2` functions (e.g. in this case we have `__hadd2(in_array[idx], __float2half2_rn(1.0))` where 1 is the constant). 

**==These constant are with `operandType::IMM_DOUBLE` and `operandType::IMM_UINT64`.==**

The final result are stored in the first two operands (**==same value==**) as FP16 formats.
e.g.

````
After   HADD2 R7, R7, 1, 1 ;, 4.500000, 1.500000,4.500000,1.500000, 0.000000, 0.000000, 0.000000, 0.000000
````

#### Questions 
- [ ] Can #(operands) = 4 implies the destination registers store two FP16?
- [ ] HMMA only have HMMA.FP32?