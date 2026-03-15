# **Data Types**
|                      |                  |                   |
| -------------------- | ---------------- | ----------------- |
| **Type**             | **Size (bytes)** | **Value**         |
| char                 | 1                | 0~255 or -128~127 |
| int(signed/unsigned) | 4                | -2**31 ~ 2**31-1  |
| float                | 4                |                   |
| double               | 8                |                   |
| long long            | 8                |                   |
| short int            | 2                |                   |
`sizeof()`
## int size
cstdint: int8_t, int16_t, uint8_t, uint16_t…
size_t
## float
`2.34E+10f: 2.34*10^10`
sign(1 bit), exponent(8 bits), fraction(23 bits): $(-1)^{sign}\cdot2^{(exponent)_2-127}\cdot(1.fraction)_2$﻿
`const` readonly
# Libraries
```C
#include <stdio.h>
#include <string.h>
#include "myheaderfile.h"
```
# Flow Control
```C
for(initializer ; condition ; increment) {
    ...
}

for(auto& elem : vector) {
    ...
}

switch(expression) {
    case value:
        ...
        break;
    default:
        ...
}
```
# Functions
## `printf`
`int printf(const char *format, ...);`
`%[flags][width][.precision][length]specifier`

|**Specifier Type**|**Format**|
|---|---|
|character|%c|
|integer|%d|
|float|%f|
|string|%s|
|memory address|%p|
## main
```C
void main() {
    printf("Hello, world!");
    return;
}
```
## lambda
```C++
std::function<int(int, int)> addTwo = [](int a, int b) { return a + b; };
```
捕获列表：
```C++
按值捕获： [=] 捕获当前作用域中的所有变量，这些外部变量不可被修改
按引用捕获： [&] 捕获当前作用域中的所有变量，这些外部变量可以被修改
x按值捕获，y按引用捕获 [x, &y]
```
# **Preprocessor directives/Macros**
```C
\#define PI 3.14
\#define DOUBLEPI (PI * 2)
\#define MULTIPI(x) (PI * X)
\#ifdef
\#endif
```
# **Pointers**
`int *myPointer;`
`int myValue = 10;`
`myPointer = &myValue;`
`*myPointer += *myPointer;`
`// myValue equals 20`
  
`foo->bar` is equivalent to `(*foo).bar`
`foo[2]` is equivalent to `*(foo+2)`
## Dynamic Memory

```C++
void* malloc(size_t size):
allocate block of memory of size bytes
void* calloc(size_t num, size_t size):
allocate block of memory for array of num elements, each size bytes long
void free(void* ptr):
free memory, one for each pointer we allocate
size should be computed using sizeof operator: sizeof(int)
```