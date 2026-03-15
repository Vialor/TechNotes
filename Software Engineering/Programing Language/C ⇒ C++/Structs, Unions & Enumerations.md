Structures: Ignoring struct padding, memory space of members are next to each other
```C
struct Point {
    int x;
    int y;
};
...
struct Point p;
p.x; p.y;
typedef
struct {
    int x;
    int y;
} Point;
// allow us to use: `Point p`, instead of `struct Point p`
// C++ does not need typedef 
```
Union: Members use the same memory space
```C++
union ipv4 {
	uint32_t address32;
	uint8_t address8[4];
};
```
Enum
```C++
enum color = {RED, YELLOW, ORANGE, BLUE, GREEN, PURPLE, NUM_COLORS}
enum color pen_color = ORANGE
```