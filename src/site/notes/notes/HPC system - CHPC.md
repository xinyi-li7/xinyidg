---
{"dg-publish":true,"dg-path":"Blogs/HPC system - CHPC.md","permalink":"/blogs/hpc-system-chpc/","dgPassFrontmatter":true,"noteIcon":"","created":"2024-04-18T19:07:10.000-07:00","updated":"2024-10-08T22:54:24.946-07:00"}
---

## Basic commands
#### Check your account info
```
myallocation
```
#### To view the current home directory usage and quota status
```
mydiskquota
```
#### View the module list
```bash
module avail
module spider openmpi
```
#### Got a running shell
```
srun -M notchpeak --account=owner-gpu-guest --partition=notchpeak-gpu-guest --nodes=1 --ntasks=1 --gres=gpu:a100 --pty /bin/bash -l
```
#### Check job's status
```
squeue -u u1266620
```
- link: https://www.chpc.utah.edu/documentation/software/slurm.php#squeue
#### Check the node state
```bash
scontrol show nodes notch001,notch002,notch003,notch204 # From chpc V100 GPUs' info
```
## Install
[[notes/HPC system CHPC- install spack\|HPC system CHPC- install spack]]

## Use container -- Singularity
[[notes/HPC system - CHPC\|HPC system - CHPC]]