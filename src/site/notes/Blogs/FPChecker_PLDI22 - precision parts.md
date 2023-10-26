---
{"dg-publish":true,"permalink":"/blogs/fp-checker-pldi-22-precision-parts/","dgPassFrontmatter":true}
---

### What we could tell: 
Before we porting the code to GPU, we could apply FPSan to the original single and double version codes to analyze the potential dangerous raised in lower precision:   
1. The location detected events in single but not in double
For LULESH

| Events              | counts | lines                                        |
| ------------------- | ------ | -------------------------------------------- |
| infinity_neg        | 3      | 1807,1845,1882                               |
| cancellation        | 6      | 824,832,840,841,817,816                      |
| latent_infinity_pos | 3      | 1854,1817,1779                               |
| latent_infinity_neg | 9      | 1843,1807,1877,1845,1882,1805,1840,1880,1802 |
| latent_underflow                    |  16      |                   1854,985,983,1817,984,979,981,980,680,691,1201,702,1779,1193,2477,1197                           |


For others: 
2.  The different special events (`["infinity_pos","infinity_neg","nan","underflow","latent_infinity_pos","latent_infinity_neg","latent_underflow"]`) in single and double (Notice that, even if there exists the same events for the same location for double and single, as long as their occurring number is different, we say they are different events)

| Events              | counts | lines                                        |
| ------------------- | ------ | -------------------------------------------- |
| infinity_neg        | 3      | 1807,1845,1882                               |
| latent_infinity_pos | 3      | 1854,1817,1779                               |
| latent_infinity_neg | 9      | 1843,1807,1877,1845,1882,1805,1840,1880,1802 |
| latent_underflow                    |  16      |                   1854,985,983,1817,984,979,981,980,680,691,1201,702,1779,1193,2477,1197                           |

We noticed that the different special events are consistent with the added locations. Thus, ....
3.. Find the overlap with the histogram
	We now compute the high exponent in double (define as `e<=-127 or e>=128 and e!=-1023`), find the union locations of it and the new special events lines

| Lines | dangerous exponent                                         | added events                          |
| ----- | ---------------------------------------------------------- | ------------------------------------- |
| 983   | -128, -127, -130, -129                                     | latent_underflow                      |
| 691   | -128, -127, -147, -146, -145, -144, -136, -135, -132, -129 | latent_underflow                      |
| 980   | -128, -127, -145, -130, -129                               | latent_underflow                      |
| 1877  | -                                                          | latent_infinity_neg                   |
| 979   | -128, -127, -146, -145, -130, -129                         | latent_underflow                      |
| 1840  | -                                                          | latent_infinity_neg                   |
| 702   | -128, -127, -147, -146, -145, -144, -136, -135, -132, -129 | latent_underflow                      |
| 680   | -128, -127, -147, -146, -145, -136, -135, -132, -129       | latent_underflow                      |
| 1802  | -                                                          | latent_infinity_neg                   |
| 985   | -128, -127, -130, -129                                     | latent_underflow                      |
| 1807  | 128                                                        | latent_infinity_neg, infinity_neg     |
| 1882  | 128                                                        | latent_infinity_neg, infinity_neg     |
| 1779  | -                                                          | latent_infinity_pos, latent_underflow |
| 1193  | -135, -134, -133, -132, -131                               | latent_underflow                      |
| 1197  | -135, -134, -133, -132, -131                               | latent_underflow                      |
| 1880  | -                                                          | latent_infinity_neg                   |
| 984   | -128, -127, -130, -129                                     | latent_underflow                      |
| 1854  | -                                                          | latent_infinity_pos, latent_underflow |
| 1845  | 128                                                        | latent_infinity_neg, infinity_neg     |
| 1843  | -                                                          | latent_infinity_neg                   |
| 1805  | -                                                          | latent_infinity_neg                   |
| 1387  | -144, -142, -139                                           | -                                     |
| 2477  | -                                                          | latent_underflow                      |
| 981   | -128, -127, -144, -130, -129                               | latent_underflow                      |
| 1817  | -                                                          | latent_infinity_pos, latent_underflow |
| 1201  | -135, -134, -133, -132, -131                               | latent_underflow                      |

4 . Modify the corresponding lines in 1,2,3, the exceptions are reduced