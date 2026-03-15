# C
```C
// ALLOCATE
void* malloc(size_t size); // allocate `size`  bytes of uninitialized storage
int* p1 = (int*) malloc(4);
// DEALLOCATE
void free(void* ptr);
```
**Memory Leak**: A piece of memory has no pointer pointing to
# C++
```C++
// ALLOCATE
int* p1 = new int; // allocate an int
int* p2 = new int(x); // allocate an int and initialize to x, default 0
int* p2 = new int{x}; // C++11 // allocate an int and initialize to x, default 0
Student* ps1 = new Student; // allocate an int, default initializer
Student* ps2 = new Student {"Yu", 2001, 1}; // allocate an int, initialize the members
int* pa1 = new int[16]; // allocate 16 int
int* pa1 = new int[16](x); // allocate 16 int and initialize all to x, default 0
int* pa1 = new int[16]{1, 2, 3}; // C++11 // allocate 16 int, first three are 1,2,3, rest 0
Student* psa1 = new Student[16];
Student* psa2 = new Student[16]{{"Yu", 2001, 1}, {"Li", 2000, 1}};
// DEALLOCATE
delete p1; // deallocate memory
delete ps1; // deallocate memory
delete pa1; // deallocate memory of array
delete []pa2; // deallocate memory of array, same as above
delete psa1; // deallocate memory of array using destructor of the first element
delete []psa2; // deallocate memory of array using specific destructor for each element
// good habit to always use `delete []` for array
```