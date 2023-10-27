---
{"dg-publish":true,"permalink":"/notes/fp-checker-and-f-li-t-analysis-tiny/","dgPassFrontmatter":true,"noteIcon":""}
---

### Clean run
Follow [[notes/FLiT -- the process to run the program you want to evaluate\|FLiT -- the process to run the program you want to evaluate]] to make a clean run, [[notes/Script to run Paranoia in both clang++12 and FPChecker from the scratch\|Script to run Paranoia in both clang++12 and FPChecker from the scratch]] may lead a quick setup. 
#### Run with FPChecker
Here, we set 
###### in the clang type, `binary=FPC_INSTRUMENT=1 clang++-fpchecker`
###### `filepath=tiny_fp.sqlite`
#### Run with clang++
Here, we set 
###### in the clang type, `binary=/home/xinyi/llvm/llvm12/bin/clang++`
###### `filepath=tiny_clang12.sqlite`
### Generate files we want to analyze
#### .fpc_logs for and `fpchecker` running
##### Use script to find the lines involve viability
The lines detected are {133, 427, 115, 538, 252}, which is corresponding to {`subnormal`,`xMinusX`,`FtoDecToF`,`xPc1EqC2`,`addSub`}
Used in [[notes/FPChecker and FLiT Analysis -- tiny#If comparison 0 not detected by FPChecker\|#If comparison 0 not detected by FPChecker]]
#### csv files
##### `tiny_fpc_non_rand.csv` and `tiny_clang_non_rand.csv`
All entries where `comparsion!=0`, remove random tests.
Used in [[notes/FPChecker and FLiT Analysis -- tiny#Compare the variability in clang and fpchecker\|#Compare the variability in clang and fpchecker]] and Used in [[notes/FPChecker and FLiT Analysis -- tiny#Run trace_compare\|#Run trace_compare]]

##### `tiny_fpc_comparsion0.csv` and `tiny_clan_comparsion.0csv`
All entries where `comparsion=0`, remove random tests.
Used in [[notes/FPChecker and FLiT Analysis -- tiny#Run trace_compare\|#Run trace_compare]]
#####


### Analysis
#### Compare the variability in `clang` and `fpchecker`
##### Hypothesis
They should be same because using FPChecker compiler doesn't influence the computations
##### Observation
They are the same after we __remove the random cases__. One thing needed to be noticed is that there's one break in clang.

#### If `comparison=0` not detected by FPChecker?
No. `addSub` is not random and the comparison is always 0, but still involve 
#### If `comparsion=0`, should there exists similar events? 
#### Run trace_compare
##### One simple case
![Pasted image 20211103120032.png](/img/user/attachment/Pasted%20image%2020211103120032.png)
![Pasted image 20211103120219.png](/img/user/attachment/Pasted%20image%2020211103120219.png)
We want to compare the files where the configuration has 1 difference but the `comparsion` change to non-zero from zero.