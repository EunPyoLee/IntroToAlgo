### Heap ###
Heap is a data structure that is useful for heapsort and makes an efficient priority queue.

The __(binary) heap__ data structure is an "array" object that we can view as a nearly "complete binary tree"
> The root of the tree is A[0], and given the index _i_ of a node, we can easily compute the indices of its parent, left child, and right child

![Alt text](../images/heap_1.png?raw=true "Title")

* if 1-indexed
```
PARENT(i)
return i/2

LEFT(i)
return 2i

RIGHT(i)
return 2i + 1
```

* if 0-idexed
```
PARENT(i)
return (i - 1) / 2

LEFT(i)
return (2 * i) + 1

RIGHT(i)
return (2 * i) + 2
```

##### max-heap #####
`A[PARENT(i)] >= A[i]`, the value of a node is at most the value of its parent (so the largest element in a max-heap is stored at the root)

#### min-heap #####
`A[PARENT(i)] <= A[i]`, the vavlue a node is at least the value of its parent (so the smallest element in a min-heap is stored at the root)

For the __heapsort algorithm__, we use __max-heaps__.

For the __priority queues__, we commonly use __min-heaps__.

### Heap high-level Analysis ###
- a heap of _n_ elements is based on a complete binary tree, its height is `O(logN)`
- So the basic operations on heaps run in time at most proportional to the height of the tree and thus take O(logN) time.
- MAX-HEAPIFY procedure, which runs in O(log N) time, is the key to maintaining the max-heap property
- BUILD-MAX-HEAP prodcedure, which runs in O(N) linear time, produces a max-heap from an "unordered array" (so better than initing priority_queue by iterating through the array and push element in it which is O(nlogn))
- MAX-HEAP-INSERT, HEAP-EXTRACT-HAX, HEAP-INCREASE-KEY, and HEAP-MAXIMUM prodcedures, which run in `O(logN)` time, allow the heap data structure to implement a `priority_queue`

### MAX-HEAPIFY ###
```
To maintain the max-heap property.
MAX-HEAPIFY assumes that the binary trees rooted at LEFT(i) and RIGHT(i) are max heaps, but that A[i] might be smaller than its children, thus "violating" the max-heap property.

//0-indexed version
MAX-HEAPIFY(A, i)
1   l = i * 2 + 1 // LEFT(i)
2   r = i * 2 + 2 // RIGHT(i)
3   largest_idx = i
4   if l < A.length and A[l] > A[i]
5       largetest_idx = l
6   if r < A.length and A[r] > A[i]
7       largest_idx = r
8   if largest_idx != i //violating max-heap invariant so exchange to maintain
9     swap(A[i], A[largest_idx])
10    MAX-HEAPIFY(A, largest_idx) // now we can guarantee that our heap array from 0 to largest_idx is maintaining max-heap property, so check the next section

/*
O(h) == O(logN) time as each recursive call takes O(1) operation and we are checking from root to the downward straight (and the tree is complete binary tree)
O(h) == O(logN) space for stack frames
*/
```

### BUILD-MAX-HEAP ###
```
/*
Bttom-up manner to convert an array A[0...n-1] into a max-heap.
The elements in the subarray A[(n/2)...n-1] are all leaves of the tree, and so each is a 1-element heap to begin with.
The BUILD-mAX-HEAp goes through the remaining nodes of the tree(non-leaf nodes) and runs MAX-HEAPIFY on each one from the lowest level.
*/
BUILD-MAX-HEAP(A)
1   A.heap-size = A.length
2   for i = (A.length / 2) - 1 down to 0:
3     MAX-HEAPIFY(A, i)

/* O(N) many times calls on MAX-HEAPIFY which is O(logN) so time compelxity is O(NlogN)
The tigher bound relies on the properties that an _n-element_ heap has height log(n) and at most `n / (2^(h))` nodes of any height h.
However, more strictly, the time required by MAX-HEAPIFY when called on a node of height _h_ is `O(h)`, and so we can express the total cost of BUILD-MAX-HEAP as being bound from above by
```
SUM[h=0 to logN] (n / 2^h)*O(h) == O( N * SUM[h=0 to logN] (h / 2^h))
== O(N * SUM[h=0 to INFINITE](h / 2^h))
== O(N)

Hence, we can build a max-heap or /min-heap from an unordered array in "LINEAR TIME"

STRICT TIME COMPLEXITY: O(N)
```
*/
```
