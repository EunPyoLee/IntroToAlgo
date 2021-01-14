### Quicksort ###
Worst-caseL: O(N^2)
But quicksort is often the best practical choe for osrting because it is remarkably efficient on the averarge: O(NlogN) with the small constant factors hidden in the O(NlogN)
Leverages: `Partioning` and `Divide-And-Conquer`

### Description of Quicksort ###
Applies the `Divide-And-Conquer` paradigm: _three-step divdie-and-conquer_ process for sorting a typical subarray A[l...r]

__Divide__: 

Partition (rearrange) the array A[l...r] into __two__(possibly empty) subarrays A[l...p-1] and A[p+1...r] (pvioted by p) such that each element of A[l....p-1] is less than or equal to A[p], which is less than or equal to each element of A[p+1.....r].

__Conquer__:

Sort the two subarrays A[l...p-1] and A[p+1...r] by recursive calls to quicksort

__Combine__

Because the subarrays are already sorted, no wowrk is needed to combine them: the entire array A[l...r] is now sorted


```
QUICKSORT(A,l,r)
1   if l == r
2     return
3   p = PARTITITON(A,l, r)
4   QUICKSORT(A, l, p - 1)
5   QUICKSORT(A, p + 1, r)

PARTITION(A, l, r)
1   parititon_val = A[r - 1] //initial pivot val is the last value
2   pivot = l; //initial pivot val is the leftmost element
3   for i = l to r - 2 //iterate through from the left to one before the last element
4       if A[i] <= partition_value
5           swap(A[pivot], A[j]) //Since we want elements smaller than partion_value on the left of the pivot and biggers on the right of the pivot
6           ++pivot
7   swap A[pivot] with A[r - 1] //plce that partition_value to the pivot which we used to partition the array into two
8   return pivot
//runtime of PARTITION: O(N)
```
The running time of quicksort depends on whether the partitioning is balanced or unbalanced, which in turn depends on which lements are used for partitioning

__if balanced__:

The algorithm runs asymptotically as fast as merge sort `O(NlogN)`

__if unbalanced__:

runs asymptotically as slowly as insertion sort `O(N^2)`



### Worst-case partitioning ###
This occurs when the partitioning routine produces one subproblems with n1 elements and on with 0 elements.
e.g) our pick for the partition_value makes one side empty and the other side n -1 many

Lets assume that this sort of the worst unbalanced partitioning scenario keep arise in each recursive call. Then:

`T(N) = T(N-1) + ON)`

If we sum the costs incurred at each level of the recursion, we get an `arithmetic series`, which evaluates to `O(N^2)`
```
An=A1 + (n-1)d
An	=	the nᵗʰ term in the sequence
A1	=	the first term in the sequence
d	=	the common difference between terms
```

But things you have to notice is that this kind of worst case happens when the input array is __already completely sorted__,
which insertion sort runs in `O(N)` time

### Best-case partitioning ###

This occurs when PARTITION produces two subproblems, each off size no more tha n/2, since one is of size floor(n/2) and one of size ceiil(n/2) - 1 (super balanced)
In this case, quicksort runs much faster

`T(N) = 2T(N/2) + O(N)`

by the master theorem, this is `O(NlogN)`


### Balanced Partitioning ###
The _average-case_ running time of quicksort is much closer to the best case than to the worst case.
Anyway, we saw that balanced partitioning is the key to generate a better result in quicksort and so important.
Therefore, picking a good pivot directly impacts the speed of the quicksort.

