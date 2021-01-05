## **Insertion sort** is an efficient algorithm for sorting a "small number of elements"

Similar to the way many people sort a hand of playing cards.
> 1. Start with an empty left hand and the cards face down on the table
> 2. Remove one card at a time from the table and insert it into the correct position in the left hand
> 3. To find the correct position for a card, compare it with each of the cards already in the hand, from right to left
> * At all times, the cards held in the left hand are sorted, and these cards were originally the top cards of the pile on the table

**Example 5 2 4 6 1 3**
>left | 5 2 4 6 1 3 (right)

> 5 | 2 4 6 1 3

> 5 compare 2 (2 gets inserted after very beginning) | 4 6 1 3

> 2 5 compare 4(4 gets inserted after 2)  | 6 1 3

> 2 4 5  compare 6 |   1 3

> 2 4 5 6 compare 1 |  3

> 1 2 4 5 6 compare 3 |

> 1 2 3 4 5 6 |

**Pseudo**

```
//Using 0-indexed
INSERTION-SORT(A)
1 for i = 1 to A.length - 1
2     pick = A[i]
3     j = i - 1
4     while j >= 0 and A[j] > pick
5          A[j + 1] = A[j] //overwite to the right(move the the right)
6          --j
7     A[j + 1] = pick
O(N^2) time and O(1) space
```

## **Selection Sort Exercise 2.2-2**

Consider sorting n numbers stored in array A by first finding the smallest element of A, and exchanging it with the element in A[1].
Then find the second smallest element of A, and exchange it with A[2]. Continue in this manner for the first n - 1 element of A.
```
//Using 0-indexed
SELECTION-SORT(1)
1 for i = 1 to A.length - 1
2   minIdx = i
3   j = i + 1
4   while j < A.length
5      if A[minIdx] > A[j]
6          minIdx = j
7      ++j
8   swap(A[i], A[minIdx])
// O(N^2) time and O(1) space
```


## **Merge Sort 2.3**

Some recursive algorithm follow a *divide-and-conquer* approach.

**Divide-and-Conquer**: breaks the problem into several subproblems that are similar to the original problem but smaller in size, solve the subproblems recursively, and then combine these solutions to createa solution to the original problem.
Technically, it is composed of **Divide**(breaking) **Conquer**(subproblem solving) **Combine**(solutions to the subproblems into the solution for the original problem)

In "merge sort",

**Divide**: Divide the n-element sequence to be sorted into two subsequences of n/2 elements each

**Conquer**: Sort the two subsequences recursively using merge sort

**Combine**: Merge the two sorted subsequences to produce the sorted answer

```
//Combine's key operation: merging two sorted subproblem results into one
MERGE(A, l, r, pivot)//r is one after the good data of current subarray
1  len1 = pivot - l
2  len2 = r - pivot
3  Create tempArr1[len1] and tempArr2[len2]
4  for i = 0 to len1 - 1
5     tempArr1[i] = A[l + i]
6  for i = 0 to len2 - 1
7     tempArr2[i] = A[pivot + i]
8  i = 0, j = 0, k = l
9  while i < tempArr1.length or j < tempArr2.length
10    if i < tempArr1.length and j < tempArr2.length
11         if tempArr1[i] < tempArr2[j]
12              A[k] = tempArr1[i]
13              ++i
14         else
15              A[k] = tempArr2[j]
16              ++j
17    else
18         if i < tempArr1.length
19              A[k] = tempArr1[i]
20              ++i
21         else
22              A[k] = tempArr2[j]
23              ++j
24    ++k

O(N) time and O(N) space where N is the len1 + len2 or the subarray total length


MERGE-SORT(A, l, r)
1  if l == r
2     return
2  m = (r - l) / 2 + l
3  MERGE-SORT(A, l, m)
4  MERGE-SORT(A, m, r)
5  MERGE(A, l, r, m)

T(N) = T(n/2) + T(n/2) + O(N) + O(1)-dividing(m finding)
         T(n/2) to solve one subproblem of size n/2 since we divide into 2 and so each level has 2 of these T(n/2
         O(1) for dividing (finding m and dividing)
         O(N) for merging
      = 2T(n/2) + cn
      
 Costs across each level of tree:
 > the top level tree has total cost cn
 > the next level down has total cost c(n/2) + c(n/2) = cn
 > the level after that has c(n/4) + c(n/4) + c(n/4) + c(n/4) = cn and so on
 > In general, the level i below the top has 2^i nodes (since this recursive tree we dividing into two and expanding by the factor of 2), each contributing a cost of c(n/2^i)
 > so that the ith level below the top has total cost 2^ic(n/2^i) = cn
 > In this problem, the total number of levels of the recursion tree(tree depth/height) is log(n)
 > To summarize, our recursive tree has log(n) levels, each costing cn, for a total cost of cn(log(n)) = cnlog(n)
 
 O(NlogN) time complexity O(N) space (temp buffer used in the merge step)
```
