---
{"dg-publish":true,"dg-path":"Blogs/Warp Matrix Functions.md","permalink":"/blogs/warp-matrix-functions/","dgPassFrontmatter":true,"noteIcon":"","created":"2023-07-05T16:03:36.000-06:00","updated":"2023-12-03T11:45:09.437-07:00"}
---


### Main functions, types 
```cpp
template<typename Use, int m, int n, int k, typename T, typename Layout=void> class fragment;

void load_matrix_sync(fragment<...> &a, const T* mptr, unsigned ldm);
void load_matrix_sync(fragment<...> &a, const T* mptr, unsigned ldm, layout_t layout);
void store_matrix_sync(T* mptr, const fragment<...> &a, unsigned ldm, layout_t layout);
void fill_fragment(fragment<...> &a, const T& v);
void mma_sync(fragment<...> &d, const fragment<...> &a, const fragment<...> &b, const fragment<...> &c, bool satf=false);
```