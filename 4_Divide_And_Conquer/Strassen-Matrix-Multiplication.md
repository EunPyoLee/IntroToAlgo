### Trivial Square matrix Multiplication - O(n^3) ###
```
SQUARE-MATRIX-MULTIPLICATION(A,B)
1   n = A.rows
2   let C be a new n x n matrix
3   for i = 0 to n - 1
4     for j = 0 to n - 1
5       c[i][j] = 0
6       for k = 0 to n - 1
7         c[i][j] += A[i][k] * B[k][j]
8   return C

```

### Strassen's Recursive Algorithm for multiplying n x n matrix O(n^(log7)) == O(N^2.81) ~= O(N^2) ###
Not Strassen's Recursive Algorithm but some recursive algorithm to multiply square matrices.
Idea:
> In each divide step, we will divide n x n matrices into four n/2 x n/2 matrices

```
SQUARE-MATRIX-MULTIPLY-RECURSIVE(A,B, ar, ac, br, bc, n)
//Assuming our n is power of 2
1   if n == 0
2     return {}
3   let C be a new n x n matrix
4   if n == 1
5     C[0][0] = A[ar][ac] * B[br][bc]
6   else
7     let TEMP1, TEMP2 be a size of n/2 x n/2 matrix
8     TEMP1 = SQUARE-MATRIX-MULTIPLY-RECURSIVE(A, B, ar, ac, br, bc, n/2)
9     TEMP2 = SQUARE-MATRIX-MULTIPLY-RECURSIVE(A, B, ar, ac + n/2, br + n/2, bc, n/2)
10    TEMP1 += TEMP2
11    for i = 0 to n/2
12      for j = 0 to n/2
13          C[i][j] = TEMP1[i][j]
14    TEMP1 = SQUARE-MATRIX-MULTIPLY-RECURSIVE(A, B, ar, ac, br, bc + n/2, n/2)
15    TEMP2 = SQUARE-MATRIX-MULTIPLY-RECURSIVE(A, B, ar, ac + n/2, br + n/2, bc + n/2, n/2)
16    TEMP1 += TEMP2
17    for i = 0 to n/2
18      for j = 0 to n/2
19          C[i][j + n /2] = TEMP1[i][j]
20    TEMP1 = SQUARE-MATRIX-MULTIPLY-RECURSIVE(A, B, ar + n/2, ac, br, bc, n/2)
21    TEMP2 = SQUARE-MATRIX-MULTIPLY-RECURSIVE(A, B, ar + n/2, ac + n/2, br + n/2, bc, n/2)
22    TEMP1 += TEMP2
23    for i = 0 to n/2
24      for j = 0 to n/2
25          C[i + n /2][j] = TEMP1[i][j]
26    TEMP1 = SQUARE-MATRIX-MULTIPLY-RECURSIVE(A, B, ar + n/2, ac, br, bc + n/2, n/2)
27    TEMP2 = SQUARE-MATRIX-MULTIPLY-RECURSIVE(A, B, ar + n/2, ac + n/2, br + n/2, bc + n/2, n/2)
28    TEMP1 += TEMP2
29    for i = 0 to n/2
30      for j = 0 to n/2
31          C[i + n/2][j + n /2] = TEMP1[i][j]
32    return C

/*
Each recursive call multiplies two n/2 x n/2 matrices, thereby contributing T(N/2) to the overall running time.
And there are total 8 of this recursive calls in each level --> 8T(N/2)
In the matrix addition step(line 7 - 31), each of these matrices contains (N^2)/4 entries, and so each of the four matrix additions takes O(n^2) time
T(N) = 8T(N/2) + O(N^2) if n > 1
      else O(1)
By the master theorem (3rd case a > b^d), this is equivalent to O(N^(log[Base2](8))
== O(N^3) which is same as the simple version
*/
```

##### Strassen's Method #####
idea:
> To make the recursion tree slightly less bushy
> Instead of performing eight recursive multiplications of n/2 x n/2 matrices, it performs only "seven"
> 1. Divide the input matrices A and B and output matric C into n/2 x n/2 submatrices. This step takes O(1) time by index calculation, just as in our plain recursive solution above
> 2. Create 10 matrices S1,S2,...,S10, each of which is n/2 x n/2 and is the sum or difference of two matrices created in step 1. We can create all 10 matrices in O(n^2) time
> 3. Using the submatrices created in step 1 and the 10 matrices created in step 2, recursively compute seven matrix products P1, P2, ..., P7. Each matrix Pi is n/2 x n/2.
> 4. Compute the desired submatrices C[0`][0`], C[0`][1`], C[1`][0`], C[1`][1`] of the result matrix C by adding and subracting various combinations of the Pi matrices. We can compute all four matrices in O9n^2) time

With this algorithm, the running time becomes
T(N) = O(1) if n == 1
       7T(N/2) + O(N^2) OW
By the Master Theorem(3rd case a > b^d), this is O(n^(log[Base2](7)) == O(N^(log7))

`STEP2 Detail O(N^2) time`

S1 = B[0'][1'] - B[1'][1']

S2 = A[0'][0'] + A[0'][1']

S3 = A[1'][0'] + A[1'][1']

S4 = B[1'][0'] - B[0'][0']

S5 = A[0'][0'] + A[1'][1']

S6 = B[0'][0'] + B[1'][1']

S7 = A[0'][1'] - A[1'][1']

S8 = B[1'][0'] + B[0'][0']

S9 = A[0'][0'] - A[1'][0']

S10 = B[0'][0'] + B[0'][1']

`Step3 Detail`

P1 = A[0'][0'] * S1 = A[0'][0'] * B[0'][1'] - A[0'][0'] * B[1'][1']

P2 = S2 * B[1'][1'] = A[0'][0'] * B[1'][1'] + A[0'][1'] * B[1'][1']

P3 = S3 * B[0'][0'] = A[1'][0'] * B[0'][0'] + A[1'][1'] * B[0'][0']

P4 = A[1'][1'] * S4 = A[1'][1'] - A[1'][1'] * B[0'][0']

P5 = S5 * S6 = A[0'][0'] * B[0'][0'] + A[0'][0'] * B[1'][1'] + A[1'][1'] * B[0'][0'] + A[1'][1'] * B[1'][1']

P6 = S7 * S8 = A[0'][1'] * B[1'][0'] + A[0'][01] * B[1'][1'] - A[1'][1'] * B[1'][0'] - A[1'][1'] * B[1'][1']

P7 = S9 * S10 = A[0'][0'] * B[0'][0'] + A[0'][0'] * B[0'][1'] - A[1'][0'] * B[0'][0'] - A[1'][0'] * B[0'][1']

