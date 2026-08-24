# Definition
**Priority Queue** (usually implemented with **Heap**. Heap is usually implemented with binary heap which is again usually implemented with array)

> **Heap:** a specialized tree-based data structure that satisfies the **heap property**: In a _max heap_, for any given node C, if P is a parent node of C, then the key of P is greater than or equal to the key of C. In a min heap, the key of P is less than or equal to the key of C.

Function-wise, PQ is more powerful than stack and queue because stack and queue are just PQ whose priority is the insert order.
**Interface Definition:**
`is_empty` O(1)
`insert_with_priority` O(logn)
`pull_highest_priority_element` O(logn)
`peek_highest_priority_element` O(1)
**Additional Operations:**
`heapify` O(n)
`sift_up` `sift_down` O(logn)
# Python Implementation
```Python
# Priority Queue implemented with Heap
import heapq

minheap = [...]
heapq.heapify(minheap)
heapq.heappop(minheap)
heapq.heappush(minheap, e)
heapq.heapreplace(heap, item) # pop + push
```
# Applications
For a stream of data, we can:
	use a min-heap and a max-heap of roughly the same size to maintain the medium of data stream
	use a min-heap of size k to maintain the max k elements; to extract max of the max k elements, we need another max-heap storing the same content of the min-heap and augment both heaps to access each other's location of the same element
