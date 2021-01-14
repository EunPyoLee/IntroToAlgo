### HeapSort ###
Heapsort algorithm leverages `Build-MAX-HEAP`(Heapify) to buid a max-heapo n the input array A[0 ... n - 1], where n = A.length
Since it is possible that children of the root remain max-heaps but the new root element might violate the max-heap property, we need to restore the max-heap property.

e.g) MAX-HEAPIFY(A, 1) leaves a max-heap in A[0.... n-2], so the heapsort algorithm repeats this process for the max-heap of size n - 1 down to a heap of size 2.

```
HAEPSORT(A)
1   BUILD-MAX-HEAP(A)
2   for i = A.length - 1 downto 0
3     exchange A[0] with A[i] // exchange the max val resulted from heapify(is front val) with the current tail point
4     A.heap-size = A.heap-size - 1 // now the remaining part to be continued to processed is 0....curlen - 1
5     MAX-HEAPIFY(A, 0)

/*
Time Complexity: BUILD-MAX-HEAP(which is heapify on an array) is O(N) and we call n-1 many of it on rest each of which takes O(logN)
so, O(NlogN)
*/
```

##### side note ######
A good implementation of quicksort usually beats heapsort in practice.
However,the heap data structure has many uses case, and one of the most popular applications of a heap is an efficient `priority queue`
