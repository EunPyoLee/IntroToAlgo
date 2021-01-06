### Brute-Force Solution
Similar to "Two Sum" brute force way, which is trying every possible pair of buy and sell.
binomial(n 2) pairs of dates == O(N^2)

### Divide-And-Conquer Solution
Idea:
Let say A[i...j] represents the maximum subarray
> There are three zones that this A[i....j] can sit:
> 1. entirely in the subarray A[left...mid], so that left <= i <= j < mid
> 2. entirely in the subarray A[mid...right], so that mid <= i <= j < right
> 3. crossing the midpoint, so that left <= i <= mid <= j < right
>> In that sense, we can recursively break down the problem into subproblems of left and right section, then solve those two subproblems and the crossing case. At the end, the original problem would be able to gather all of sub results and return the answer.

'''
// Caculates the crossing zone and return the range of this zone with the SUM value
FIND-MAX-CROSSING-SUBARRAY(A, left, mid, right)
1   cur_sum = 0, begin = mid, end = mid
2   left_max = INT_MIN, right_max = INT_MIN
3   for i = mid - 1 downto left  
4     cur_sum += A[i]
5       if cur_sum > left_max
6         left_max = cur_sum
7         begin = i
8   cur_sum = 0
9   for i = mid to right - 1
10    cur_sum += A[i]
11    if cur_sum > right_max
12      right_max = cur_sum
13      end = i
14  return (begin, end, left_max + right_max)
//O(N) time O(1) space where N is the length of (right - left)

// Higher-Level function: dividing into subproblems solves them and solve the crossing case
FIND-MAXIMUM-SUBARRAY(A, left, right)
1   if left == right
2     return (left, right, 0) //empty
3   mid = (right - left) / 2 + left
4   (left_left, left_right, left_sum) = FIND-MAXIMUM-SUBARRAY(A, left, mid)
5   (right_left, right_right, right_sum) = FIND-MAXIMUM-SUBARRAY(A, mid + 1, right)
6   (cross_left, cross_right, cross_sum) = FIND-MAX-CROSSING-SUBARRAY(A, left, mid, right)
7   if left_sum > right_sum AND left_sum > cross_sum
8      return (left_left, left_right, left_sum)
9   else if right_sum > left_sum AND right_sum > cross_sum
10     return (right_left, right_right, right_sum)
11  else
12     return (cross_left, cross_right, cross_sum)

/*
Base Case (n == 0): T(0) = O(1) constant
Recursive Case (N > 0): 
  we divide into two subproblems(T(N/2)) and solves each of them (2 * T(N/2)) and we also solve the crossing case (O(N))
  T(n) = 2T(N/2) + O(N) (the recurrence is the same as recurrence for merge sort)
  
  ***
  Master Theorem:
    T(N) = aT(n/b) + f(n)
    T(1) = c
    where a>= 1, b>=2, c > 0. If f(n) belong st o n^d where d>= 0, then
    T(N) =
      1. O(n^d) if a < b^d
      2. O(n^d log(n)) if a = b^d
      3. O(n^(log[baseb](a)) if a > b^d
  
  So by the Master Theorem, our case is second case as our a == 2 and b^d == 2^1 == 2.
  Thererfore, T(N) = O(N^1 log(N)) == O(NlogN)
  
  Time Complexity: O(NlogN)
  Space Complexity: O(logN) - recursive tree depth(# of stack frame we used)
*/
'''
