---
{"title":"FPChecker Installation","tags":["install","FPChecker","quick_setup"],"date":"2022-05-12","dg-publish":true,"dg-path":"Blogs/FPChecker Installation.md","permalink":"/blogs/fp-checker-installation/","dgPassFrontmatter":true,"noteIcon":""}
---


* tag: #Project 
* Link: https://fpchecker.org/installation.html

#### A practical script

Install in Attenborough

1. You need  to install llvm, see [LLVM - installation](LLVM%20-%20installation.md) to install the llvm thoroughly. It seems version 10, 11, 12 are OK
1. Download FPchecker

````bash
git clone --recursive https://github.com/LLNL/FPChecker.git
cd FPChecker
mkdir FPChecker_install
````

3. Change the Cmake file
   Change the fist few lines in [FPchecker - CMakeLists.txt > Code](FPchecker%20-%20CMakeLists.txt.md#code), see reason [here](FPchecker%20-%20CMakeLists.txt.md#reason)
3. Build

````bash
mkdir build
cd build
cmake -DCMAKE_INSTALL_PREFIX=../FPChecker_install .. 
make
make install
````

5. Add binary to `PATH` in `~/.bashrc`

````bash
export PATH=/home/xinyi/FPChecker/FPChecker_install/bin:$PATH
````

6. Set 
6. Test (need install pytest by pip)

````bash
(in build)
ctest -V
````

#### Issues

##### Cannot find `clang/AST` module - llvm install issue

Check 

````
/path/to/your/llvm/bin/llvm-config --cxxflags
````

find `clang/` are not in the include path, see solution [LLVM - installation > Cannot find the clang libraries when FPCheck Installation](LLVM%20-%20installation.md#cannot-find-the-clang-libraries-when-fpcheck-installation)

##### CPU test are all failed -- didn't specify the right `clang` compiler

In `FPChecker/cpu_checking`, modify compilers in all `*_frontend.sh` files as our compiler. 
E.g. we modify `cc_frontend` as

````bash
export FPC_COMPILER='/home/xinyi/llvm/llvm10/bin/clang'
export FPC_COMPILER_PARAMS=`printf "%q " "$@"`
DIR=`dirname $0`
exec "$DIR/../cpu_checking/clang_fpchecker.py"
````

##### MPI problem (usually `cannot find useful symbols`)

When running MPI problem, it used the default `clang++`. Thus, we need to specify the default `clang++` as the correct clang.

###### E.g. in Attenborough

````
export PATH=~/llvm/llvm12/bin:$PATH
````
