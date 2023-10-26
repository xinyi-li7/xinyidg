---
{"dg-publish":true,"permalink":"/blogs/fp-checker-issue-cannot-link-the-open-mp-lib/","dgPassFrontmatter":true}
---

- Link: https://github.com/LLNL/FPChecker/blob/master/tests/cpu_checking/dynamic/test_openmp/Makefile

##### Makefile
Just add link during linking time as the above link showed 
##### CMake (takes a lot of time to find the solution!!!!)
Add 
```
execute_process(COMMAND /home/xinyi/llvm/llvm12/bin/llvm-config --ldflags 
OUTPUT_VARIABLE CMAKE_SHARED_LINKER_FLAGS 
OUTPUT_STRIP_TRAILING_WHITESPACE)

execute_process(COMMAND /home/xinyi/llvm/llvm12/bin/llvm-config --cxxflags
OUTPUT_VARIABLE CMAKE_CXX_FLAGS
OUTPUT_STRIP_TRAILING_WHITESPACE)

execute_process(COMMAND /home/xinyi/llvm/llvm12/bin/llvm-config --cppflags
OUTPUT_VARIABLE CMAKE_CPP_FLAGS 
OUTPUT_STRIP_TRAILING_WHITESPACE)
```
To CMakeLists.txt
Refer:https://stackoverflow.com/questions/37969440/clang-openmp-and-cmake