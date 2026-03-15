# Arrays
```C++
// fixed-length
int num_arr[5] = {0, 1, 2, 3, 4};
// variable-length
int num_arr[len] // cannot be initialized
// unkown size
int num_arr[] = {0, 1, 2, 3, 4}; // depends on the initialized value
// multi-dimensional
int mat[2][3] = {{11,12,13}, {21,22,23}}
// int mat[][]; unknown bound
// int mat[][3]; correct
```
# Strings
## Initialize
array-styled strings
`char name[4] = {'B', 'o', 'b', '\0'};`
string literals
`char name[] = "Bob";`
## C Operations
deep copy
`char *strcpy(char *dest, char *src);`
`char *strncpy(char *dest, char *source, size_t);`
concat
`char *strcat(char *dest, char *src);`
`char *strncat(char *dest, char *src, size_t size);`
length
`size_t strlen(char *src);`
`int *strcmp(char *stringOne, char *stringTwo);`
## std::string
```C++
std::string str1 = "Hello ";
std::string str2 = "World";
// deep copy
str1 = str2
// concat
str1 + str2
// length
str1.length()
str1 < str2
```