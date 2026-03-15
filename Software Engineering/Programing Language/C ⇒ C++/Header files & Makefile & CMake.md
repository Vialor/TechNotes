# Header File
.h中一般放的是同名.c文件中定义的变量、数组、函数的声明，需要让.c外部使用的声明。1)h文件作用:
1. 方便开发:包含一些文件需要的共同的常量,结构,类型定义,函数,变量申明；
2. 使函数的作用域从函数声明的位置开始，而不是函数定义的位置
3. 提供接口，对一个软件包来说可以提供一个给外界的接口
  
gotcha:
1. header file可以有： 常量，结构，类型定义，函数声明，变量声明；不可以有变量定义和函数定义
2. 变量用extern，函数不用extern因为其缺省状态就是extern的，函数如果要仅文件内可见需要加static
3. 虽然申明和类型定义可以重复，但推荐使用条件编译：
    
    ```C
    \#ifndef _FILENAME_H
    \#define _FILENAME_H
    ...
    \#endif
    
    or use a non-standard but widely supported preprocessor directive:
    \#pragma once
    ```
    
# Makefile
```Makefile
## VERSION 1
# `make target`: regenerate `hello` if any dependency main.c hello.c is newer than target
hello: main.c hello.c
		gcc -o hello main.c hello.c
## VERSION 2
# only recompile changed c files
CXX = g++
TARGET = hello
OBJ = main.o hello.o
$(TARGET):$(OBJ)
		$(CXX) -o $(TARGET) $(OBJ)
main.o: main.c
		$(CXX) -c main.c
hello.o: hello.c
		$(CXX) -c hello.c
## VERSION 3
# more refined techniques
# use := as simple assignments for macros, use = as recursive assignments
# simple assignment expression is evaluated only when defined
CXX := g++
CXXFLAGS := -c -Wall # -Wall: show all warnings
TARGET := hello
OBJ := main.o hello.o
$(TARGET): $(OBJ)
		$(CXX) -o $@ $^ # $@: target; $^: dependency objects
%.o: %.c # %: wild match
		$(CXX) $(CXXFLAGS) $< $@ # $<: first dependency object
.PHONY: clean # declare that clean is a phony target; we are not actually creating a file called clean
clean:
		rm -f *.o $(TARGET)
```
`make hello` runs the line with hello as target
`make` runs the first line with a target
# CMake
Cross platform Make
1. write `CMakeLists.txt`
```C
cmake_minimum_required(3.10)
project(main)
add_executable(main main.c hello.c)
```
1. run `mkdir build && cd build && cmake ..`
This will generate: `CMakeCache.txt` , `CMakeFiles`, `Makefile`
## Library
static library: integrated at compile time; executable can run independently; large in size and multiple processes can be duplicated copies in memory
`.a` archive `.lib` library
dynamic library: integrated at runtime; executable need dynamic libraries to run; small in size and processes share a single copy in memory
`.so` shared object `.dll` dynamic link library