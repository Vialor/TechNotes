# Mapping Based Structures
## String & Array
```java
// array
int[] nums = {3, 2, 4, 1};
int[] nums = new int[amount];

nums[1] // 2
nums.length() // 4

Arrays.sort(nums); // nums {1, 2, 3, 4}
Arrays.toString(nums);
Arrays.fill(nums, 1);
Arrays.asList(1, 2, 3, 4) // quick convert to a fixed-size list

// string
s.substring(0, i)
s.substring(i)
s.charAt(i)
s.length()
```
## Collections
### Hierarchy of Collection Framework
```Java
// use ** for interface
*Collection*: add, addAll, remove, removeAll, removeIf, size, clear, contains, toArray, isEmpty, equals
	*List*
		ArrayList
		Vector
			Stack
		LinkedList
	*Queue*
		*Deque*
			ArrayDeque
			LinkedList
		PriorityQueue
	*Set*
		*SortedSet*
			TreeSet
		HashSet
		LinkedHashSet
```

```java
// List
List<String> cars = new ArrayList<>();
get, set, indexOf
List<String> cars = new LinkedList<>(); // LinkedList also implements Deque
// List deep copy
for(Person p : originalList)
    clone.add(p.clone());
// List shallow copy
List<String> cpath = new ArrayList<>(path);

// Stack
Stack<Integer> stack = new Stack<>();
push, peek, pop, isEmpty
// Queue
offer, poll, element, peek
// Deque
Queue<String> deque = new ArrayDeque<>();
addFirst, addLast, getFirst, getLast, removeFirst, removeLast
// Priority Queue
Queue<Obj> queue = new PriorityQueue<Obj> ();

// Set
HashSet<String> set = new HashSet<>();
// Hash Map
HashMap<String, Integer> map = new HashMap<>();
put, get, containsKey, containsValue
keySet, values, entrySet
// Hashmap traverse
for (Map.Entry<String, Integer> entry : map.entrySet()) {
	System.out.println("Key: " + entry.getKey() + ", Value: " + entry.getValue());
}
graph.computeIfAbsent(word, k -> new ArrayList<>()).add(neighbor);
```
# Pointer Based Structures
