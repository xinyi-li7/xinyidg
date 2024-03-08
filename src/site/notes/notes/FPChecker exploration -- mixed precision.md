---
{"dg-publish":true,"dg-path":"Blogs/FPChecker exploration -- mixed precision.md","permalink":"/blogs/fp-checker-exploration-mixed-precision/","dgPassFrontmatter":true,"noteIcon":"","created":"2023-02-20T17:56:09.976-07:00","updated":"2023-10-24T01:21:49.877-06:00"}
---

- tag: #Project 
The program we used here is LULESH
### Run FPChecker Histogram to generate the log and histogram json

### Analyze the results
#### Check the if double/single have different locations
Using `file+line` as the index, ~~there's no location difference~~ <- I got the wrong answer since I didn't notice they log every line in __histogram__ version. 
There're location differences 
see [diff_single_double.json](https://drive.google.com/file/d/1P5XxDvaW-SBnDIfYL0_BF43QOdsxRDZZ/view?usp=sharing)
##### Analysis
The results shows: `single - double != 0` and `double - single = 0`, which is reasonable since with single, we may involve more exceptions. 
##### For extra lines in single, what kinds of events involve and their corresponding counts
```
   "infinity_neg": {
        "count": 3,
        "locatio_list": ....
    "cancellation": {
        "count": 6,
        "locatio_list": ....
    },
    "latent_infinity_pos": {
        "count": 3,
        "locatio_list": ....
    },
    "latent_infinity_neg": {
        "count": 9,
        "locatio_list": ....
    },
    "latent_underflow": {
        "count": 16,
        "locatio_list": ....
    }
```

#### For the shared lines between double and single, the events difference
see [diff_single_double.json](https://drive.google.com/file/d/1P5XxDvaW-SBnDIfYL0_BF43QOdsxRDZZ/view?usp=sharing)
##### For different events in single and double, what kinds of events involve and their corresponding counts
What does it means if we have different `cancellation`?
##### Find events with large difference

#### Find the location with high density w.r.t edge exponent in double 
##### Edge exponent
We define the edge exponent as the exponent close to the limits of single. So, here we want to find lines involving exponents (>127 or < -127). In addition, if `exponent=1023` or `-1023`. It means double cannot represent it as well. So we ignore this information.
##### 
The condition we use is:
```python
if(exponent>127 or exponent < -127):
	if(exponent!=1023 and exponent!=-1023):
		add this location entry into our list
```
see [he_double.json](https://drive.google.com/file/d/19hG_tEQo3EHU1jQ46UIaAr-_bBCGzAxF/view?usp=sharing)
#### ~~Combine the information from [[notes/FPChecker exploration -- mixed precision#For the shared lines between double and single the events difference\|difference events between double and single]] and [[notes/FPChecker exploration -- mixed precision#Find the location with high density w r t edge exponent in double\|double high exponent]]~~ Overlap between high exponent lines and big_diff+added lines. 
##### lines in high exponent but not in diff lines?
All with -1023 exponent but no events detected: reasonable. When `e=-1023`, the corresponding value is 0. See [[notes/IEEE - 754 floating points#Exponent\|IEEE - 754 floating points#Exponent]]
##### Events <-> exponents


#### The differences in `he_single.txt` and `he_double.txt`
### Test using mixed precision
#### Use the [he_double.json](https://drive.google.com/file/d/19hG_tEQo3EHU1jQ46UIaAr-_bBCGzAxF/view?usp=sharing) only
For all lines in [he_double.json](https://drive.google.com/file/d/19hG_tEQo3EHU1jQ46UIaAr-_bBCGzAxF/view?usp=sharing), we do a [[notes/FPChecker exploration -- mixed precision#shallow precision change\|#shallow precision change]], and run it using mix-precision. 
##### Shallow precision change
We locate the lines in [he_double.json](https://drive.google.com/file/d/19hG_tEQo3EHU1jQ46UIaAr-_bBCGzAxF/view?usp=sharing). We find the assignment lines of the related variable manually and replace them with `double`.
##### Result
Seems not work. `single` and `mixed` has the same answer, which is not the same as `double`.
##### Thoughts
The precision changes are too shallow.(We didn't change the intermediate computations.)
##### Compare the exceptions between the above mixed version and single version
```
lines in single but not in mixed is  {'lulesh.cc 691', 'lulesh.cc 1877', 'lulesh.cc 680', 'lulesh.cc 1840', 'lulesh.cc 702', 'lulesh-util.cc 203', 'lulesh.cc 984', 'lulesh.cc 980', 'lulesh.cc 983', 'lulesh.cc 985', 'lulesh.cc 1882', 'lulesh.cc 1807', 'lulesh.cc 1845', 'lulesh.cc 1802', 'lulesh.cc 981', 'lulesh.cc 979'}

lines in mixed but not in single is  {'lulesh.cc 1147', 'lulesh.cc 1146'}
```
Do reduce some lines, but increase `underflow` and `latent_underflow` in mixed version
For now, just say the `histogram` could help reduce most exceptions, while add a small number of exceptions? 
#### Use lines in 

### Modify the lines in [[notes/IEEE - 754 floating points#Special cases\|special case]] exceptions found in single but not in double

<div class="transclusion internal-embed is-loaded"><div class="markdown-embed">



#### Definitions 
[[notes/IEEE - 754 floating-points - Different representation of a floating-point\|IEEE - 754 floating-points - Different representation of a floating-point]]
[[notes/IEEE - $e_{max}$, $e_{min}$\|IEEE - $e_{max}$, $e_{min}$]]
[[IEEE - 754 subnormal\|IEEE - 754 subnormal]]
[[notes/IEEE - Special values (NaN, INF, subnormal)\|IEEE - Special values (NaN, INF, subnormal)]]



</div></div>

#### Detect related lines
##### [[notes/FPChecker exploration -- mixed precision#The locations where only the underflow infinite Nan events are added or different\|diff_special_events_single_double.txt]] 
#####
#####

### Generated files
##### The locations appearing in single but not in double 
[diff_sinlge_double.txt] 
##### The locations appearing in double but not in single
no
~~##### The locations where the events are different (including the counts)
[diff_single_double_events.txt]~~ -> combine with [diff_single_double.txt]
See the [[notes/FPChecker exploration -- mixed precision#The locations which is newly added to single the locations where the events are different compared to double\|diff_added_single_double.txt]]
##### The events-location map where the locations are newly added to single
[NewLoc_added_single_EventTolocMap.txt]
##### The locations which is newly added to single + the locations where the events are different compared to double
[diff_added_single_double.txt]
##### The locations where only the `underflow`, `infinite`,`Nan` events are added or different
[diff_special_events_single_double.txt]
```
python3 check_plain_logs.py -s ./single/build/.fpc_logs/fpc_attenborough_26497.json -d ./double/build/.fpc_logs/fpc_attenborough_26480.json
```

##### The locations with exponent >127 or <-127 in both single and double
[he_double.txt], [he_single.txt]
```
python3 High_exponent.py ./double/build/.fpc_logs/histogram_attenborough_26480.json fp64
```
##### Different for exponents in single and double 
[diff_ex_single_double.txt]
We notice that exponent = -127 in single are regarded the same as exponent = -1023 in double. 
Difficult is compare two `fp32_dict` and `fp64_dict`
```
fp32_dict = {"-127": 6, "34":7}
fp64_dict = {"-1023": 6, "34":7,"-128":7}
```
want to return
```json
{
	"fp32":
		{
			
		}
		"fp64":
		{
			"-128":7
		}
}
```
```
exs = fp32_dict.keys().union(fp64_dict.keys()) #{"-127","-1023","34","-128"}
entry = {}
entry["fp32"] = {}
entry["fp64"] = {}
if("-1023" in exs):
	exs.remove("-1023")
for ex in exs:
	if(ex == '127'):
		if(fp32_dict.get("-127")!=fp64_dict.get("-1023")):
			entry["fp32"]["-127"] = fp32_dict.get("-127")
			entry["fp64"]["-1023"] = fp64_dict.get("-1023")
		if(fp64_dict.get("-127")!=None):
			entry['fp64']['-127'] = fp64_dict.get("-127")
	if(fp32_dict.get(ex)!=fp64_dict.get(ex)):
		entry["fp32"][ex] = fp32_dict.get("-127")
		entry["fp64"][ex] = fp64_dict.get("-1023")

```




#####  locations detected events in single but not in double with location-events map
[NewLoc_added_single_LocToEventMap.txt]
```
python3 check_plain_logs.py -s ./single/build/.fpc_logs/fpc_attenborough_26497.json -d ./double/build/.fpc_logs/fpc_attenborough_26480.json > NewLoc_added_single_LocToEventMap.txt
```

##### The union lines between [he_double.txt] and [NewLoc_added_single_LocToEventMap.txt]
```
python3 he_plain_ana.py NewLoc_added_single_LocToEventMap.json he_double.json
```
[union_diff_he.txt]
##### Count the special events in file
Single 
```
python3 check_one_log.py ./single/build/.fpc_logs/fpc_attenborough_26497.json
```
Mixed 
```
python3 check_one_log.py ./mixed/build/.fpc_logs/fpc_attenborough_29458.json
```

### What we could tell: 
[[notes/FPChecker_PLDI22 - precision parts\|FPChecker_PLDI22 - precision parts]]