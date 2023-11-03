---
{"dg-publish":true,"permalink":"/paper-summary/learning-concise-models-from-long-execution-traces/","dgPassFrontmatter":true,"noteIcon":"","created":"2023-10-30T11:57:12.636-06:00","updated":"2023-11-03T16:59:02.551-06:00"}
---

### Introduction
##### Motivation -- What can we learn from the traces
Hardware & Software are designed in different companies 
->
People want to know the **behaviors** of software on specific hardware
->
> Concise, human-readable models that express high-level hardware-software interactions can provide users with a better insight into the working of the system. This can, in turn, aid in design exploration, analysis, testing and veriﬁcation applications.

###### Example of a model (NFA) that may be learned from traces
This is a model described the slot-level operations of a USB. 
![Pasted image 20231103120840.png](/img/user/attachment/Pasted%20image%2020231103120840.png)
##### Key insights -- Why this algorithm/model is good
- Scalable -- by segmentation approach 
- Produce the ==abstract==, ==concise== model
- Can form the transition-edge predicates that are ==not explicit== in the trace. 
- Use SAT-based approach with program synthesis

### Preliminaries 
##### Terminology to define the traces 
$X=\{x_1, ..., x_k\}$: variables
$X'=\{x'_1, ..., x'_k\}$: primed variables variables (represents an ==update== to the unprimed variable $x_i$ at the end of a discrete step.)
$D$: domain $D$, where the values of variables $X$ and $X'$ settle 
$v$: valuation $X \rightarrow D$ (assign value to variable $x$)
$v_t$: observation. a valuation of the variable==s== at time step $t$
$\delta = v_1, v_2, ..., v_n$: trace.  It's a trace with $n$ observations as a sequence of valuations. 


##### The final NFA (Non-Deterministic Finite Automation) model 
$\mathcal{M} = (\mathcal{Q}, q_0, \Sigma, F, \delta)$

$\mathcal{M}$: NFA machine
$\mathcal{Q}$: finite set of states, and $q_0 \in \mathcal{Q}$ is the initial state. 
$\Sigma$: finite alphabet. A symbol $a \in \Sigma$ is $X \cup X' \rightarrow D$, i.e., a pair of observation of the system. 
The symbol $a_i$ for $i=1, ..., n-1$ is
$$
a_i(x) = v_i(x),
$$
$$
a_i(x') = v_{i+1}(x)
$$

$F\subseteq\mathcal{Q}$ is the set of accepting states.
$\delta: \mathcal{Q}\times\Sigma \rightarrow \mathcal{P}(\mathcal{Q})$: transition relation, define how one state goes to the next state. 

The automation accepts a word $w=a_1, ..., a_p$ over $\Sigma$ for $p<n$ if there exists a sequence of automaton states $q_1, ..., q_{p+1}$ such that 
- $q_1 = q_0$
- $q_{i+1} \in \delta(q_i, a_i)$ for $i=1, ..., p.$  (there is transition relation can translate one state to the next state)

### Algorithm
1. Generate Trace (is not included in the model) 
	-> 
2. Predicate Synthesizer 
	-> 
3. Construct the machine

#### Predicate Synthesizer
###### What to generate
Generate the synthesized function $next(x)$ according to the trace data, a predicate is $x' = next(x)$.

This paper is using `Fastsynth` and `CVC4` to synthesize these predicate.

Notice that, the trace is feed into the `Fastsynth` and `CVC4` by segments
![Pasted image 20231102202543.png](/img/user/attachment/Pasted%20image%2020231102202543.png)
So, the predicates are from the segments

###### Form the model (Generate the NFA)
The machine $\mathcal{M}$ is essentially an array with each element is a tuple of $(q_i, p'_i, q'_i)$
$\mathcal{M} = \{((q_1, p'_1, q'_1), (q_2, p'_2, q'_2), (q_m, p'_m, q'_m))\}$

1. Divide the predicates we obtained before according to the sliding window with length $w$. 
	- ![Pasted image 20231102203138.png](/img/user/attachment/Pasted%20image%2020231102203138.png)
2. Find the machine using CBMC to find the counterexample, the steps are:
	1. Set the constraint be no such machine exists
		- ![Pasted image 20231102203455.png](/img/user/attachment/Pasted%20image%2020231102203455.png) 
	2. Form the C program that uses the predicates from  $P$ we obtained from step 2 to fit the machine $\mathcal{M}$
	3. Use CBMC to verify this formed machine. 
		- If assertion holds, no such machine exists, we incremental the state size $N$
		- If assertion not hold, we can get the counterexample. We then need to verify if all transition sequences in $\mathcal{M}$ belong to $P$. If not, we need to add the constraints and form other C program. 
### Benchmarks
Trace2Model can generate a more simpler NFA compared to state merge methods. However, trace2model runs slower than trace2model but is more scaleable. 

### Segmentation can speed the generation of the model

Segmentation makes the generated predicates less. This can avoid some repeated information from recurring patterns. 
