```Python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next
```
# Linked List O(n) Operations
## Serialization
```Python
def serializeLinkedList(self, head: Optional[ListNode]) -> List:
    ans = []
    while head:
        ans.append(head.val)
        head = head.next
    return ans
def deserializeLinkedList(self, array: List) -> Optional[ListNode]:
    if len(array) == 0:
        return None
    return ListNode(val=array[0], next=self.deserializeLinkedList(array[1:]))
```
## Merge (in order for example)
```Python
def mergeLists(first, second):
	  if first is None:
	      return second
	  if second is None:
	      return first
	  if first.val < second.val:
	      first.next = mergeLists(first.next, second)
	      return first
	  second.next = mergeLists(second.next, first)
	  return second
```
## Reverse
```Python
def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
    newHead = None
    while head:
        nextHead = head.next
        head.next = newHead
        newHead = head
        head = nextHead
    return newHead
```
## Divide into halves
```Python
def splitLists(self, head: Optional[ListNode]):
    if head is None:
        return None, None
    slow, fast = head, head
    while fast.next and fast.next.next:
        slow = slow.next
        fast = fast.next.next
		secondHead = slow.next
		slow.next = None
		return head, secondHead # first list has 1 more node than second when odd
```
# Techniques
### Steps to change linked node’s relative relationship
1. Start from the first relevant node, assign all relevant nodes to new variables
2. Use pointers to link the node based on new relationship
3. Check if the head and tail have been handled properly
4. Optimize and simplify the code if possible
### Dummy Node
a dummy node at the front/end of the list that is there only to reduce the need for special-case code in the linked-list operations.
Often used in iterative changes (as opposed to recursive changes) in linked list. A dummy head is an exclusive upper bound. Always use it, it is much more elegant.
### Two Pointers
**fast & slow pointers**: use speed 1 and speed 2 pointers to figure out the structure of the pointer chain.
**parallel pointers**: use two speed 1 pointers to figure out the relationship between two pointer chains.
# Cycles
## Existence of a cycle
LC 141 Linked List Cycle
针对141有以下问题：
1. 为什么node的value其实是无关的？
    
    因为不同node可以有相同value，这种情况下即使遍历到以前遍历过的value也不能肯定是先前的那个node。
    
2. 为什么我们需要慢指针？
    
    数据结构分为两个部分：一般的linked list加上一个cycle。我们需要慢指针是为了能够让指针最后一定能进入cycle的部分。如果慢指针没有速度，它就会一直停留在前面linked list的部分不会进入cycle，快指针也就不能和它相遇了。如果没有慢指针，我们也没有办法得知自己什么时候进入了cycle，慢指针能确保运行到最后一定进入了cycle。
    
3. 为什么快指针的速度要是2？
    
    快指针的速度=慢指针的速度+1，这个1很重要。因为如果快指针快得比1大的话，快指针很可能超过了慢指针而自己却不知道。（我们应当注意我们检查“超过”的方式是“快指针的地址等于慢指针”）
    
    这里有一个很简单的思考方式：相对速度。当慢指针进入cycle的时候，快指针一定进入了cycle。这时我们以慢指针为参考系，那么快指针的速度就变成了1，相当于慢指针不动，快指针一步一步地接近慢指针。如果快指针速度比慢指针大2，那快指针很可能直接就越过了慢指针。
    
4. 为什么复杂度是O(n)?
    
    假设n=a+b, a是前半程linked list的部分，b是cycle的部分。慢指针进入cycle前复杂度是a。慢指针进入cycle后，快指针不可能落后慢指针超过b个node。按照3中的分析，每经过1点复杂度，快指针和慢指针的距离缩小1。这样快指针追上慢指针的复杂度小于b。因此总复杂度不超过a+b=n。
    
## Entry node of a cycle
LC 142
```Plain
def detectCycle(self, head: Optional[ListNode]) -> Optional[ListNode]:
        if head is None: return None
        fast, slow = head, head
        while fast and fast.next: 
            fast = fast.next.next
            slow = slow.next
            if fast == slow:
                slow = head
                while fast != slow:
                    fast = fast.next
                    slow = slow.next
                return fast
        return None
```