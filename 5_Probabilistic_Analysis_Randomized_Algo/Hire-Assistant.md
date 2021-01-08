```
HIRE-ASSISTANT(n)
1   best_candidate = -1
2   for i = 0 to n - 1
3       interview candidate i
4       if candidate i is better than best_candidate
5           best_candidate = i
6           hire candidate i
/*
Assuming that interviewing has a low cost, say c, whereas hiring is expensive costing C. Letting m be the number of people hired
then the runtime is O(cn + Cm).
--> No matter how many people we hire, we always interview n candidates and thus always incur the cost cn associated with interviewing.
However, the number of m isn't concrete.

Therefore in Worst-cases where we actually hire every candidate that we interviewed because the candidates come in strictly increasing order of quality,
our m == n. So the runtime is O(cn + Cn) = O(Cn)
*/
```

### Probabilistic Analysis ###
Since the m varies and we have no control over the order of candidates(which affects m), we need to use __Probabilistic Analysis__ to analyze the running time of an algorithm.

In order to perform a probabilistic analysis, we must use knowledge of, or make assumption about, __the distribution of thei nputs__. Then we analyze our algorithm, computing an average-case running time, where we take the average over the distribution of the possible inputs. Thus we are, in effect, averaging the running time over all possible inputs, When reporting such a running time, we will refer to it as the __average-case running time__.
For this hiring problem, we can assume that the applicants come in a random order. In other words, we can compare any two candidates and decide which one is better qualified; that is, there is a total order on the candidates.
So we can rank each candidate with a unique number from 1 through n. The number of possible unique set of the candidate's ranking would be a permutation of the list <1,2,...,n>. So, the ranks form a __uniform random permutation__; each of the possible n! appears with equal probability

To gain more control over input distribution, we will strongly assume that the candidates will come in a random order.

Now we call this __randomized__ because its hehavior is determined not only by its input but also by values produced by a __random-number generator__.
Let say we'll have a random number geneator called RANDOM at our disposal, where RANDOM(a,b) returns an integer [a, b)
When analyzing the runnign time of a randomized algorithm, we take the expectation of the running time over the distribution of values returned by the random number generator. So, randomized algorithms are distinguished by referring to the running time of a randimized algorithm as an __expected running time__.

* Average-case running time: we discuss this when the probability distribution is over the inputs to the algorithm
* Expected running time: we discuss this when the algorithm itself makes random choices


### Indicator Random Variable ###
Indicator Random Variables provide a convenient method for converting between probabilities and expectations.
e.g) Indicator random variable I{A} associated with event A is:

```
I{A} = 1 if A occurs,
       0 if A does not occur
```

e.g) The "expected" number of heads obtained in one flip of the coin(== expected value of our indicator random varialbe)
Let say our Indicator Random Varialbe associated with the head side == Xh
Then,
```
Xh = I{H} = 1 if H occurs
            0 if T occurs
E[Xh] = E[I{H}]
      = 1 * Pr{H} + 0 * Pr{T}
      = 1 * (1/2) + 0 * (1/2)
      = 1/2
*Given a sample space S and an event A in the sample space S, let XA = I{A}. Then E[XA] = Pr{A}. The expected value of an indicator random variable associated with an event A is equal to the probability that A occurs.
```

Indicator Random Variables are useful for analyzing situation in which we perform "repeated random trial"
e.g) let say Xi be the indicator random variable associated with the event in which the ith flip comes up heads. Xi = I{the ith flip results in the event H}. Let X be the random variable denoting the "total number" of heads in the n coin flips, so that
```
X = SUM[i=1 to n](Xi)
```
If we want to compute the expected number of heads, we take the expectation of both sides
```
E[X] = E[ SUM[i=1 to n](Xi) ]
     = SUM[i=1 to n]( E[Xi] )
     = SUM[i=1 to n)( 1/2 )
     = n * (1/2) = n/2
```
Applying the analysis using "indicator random variable" to the analysis of the hiring problem

- We assumed that candidates arrive in a random order

Let X be the random variable whose value equals the number of times we hire a new offices assistant; 
This is `E[X] = SUM[x=1 to n] (x * Pr{X == x})`
However, by using indicator random variables, instead of computing E[X] by defining one variable associated with the number of times we hire a new office assistant, we define n variables related to whether or not each particular candidate is hired.
Now, let say `Xi` be the indicator random variable associated with the event in which the ith candidated is "hired"
```
Xi =  I{candidate i is hired}
   =  1 if candidate i is hired
      0 if candidate i is not hired
And X = X1 + X2 + Xn
```

Since `E[Xi] = Pr{candidate i is hired}`, we need to figure our E[Xi] or probability of candidate i is hired.
If candidate i is hired, candidate i should be better than each of canddiates 1 to i - 1, because we are assuming that the candidates arrive in a random order and so any one of these first i candidates is equally likely to be the best-qualified so far.
Candidate i has a probability of 1 / i of being better qualified than candidates.
Then. `Ex[i] == Pr{candidate i is hired} == 1/i`
```
E[X] = E[ SUM[i=1 to n] (Xi) ] 
     = SUM[i=1 to n] E[Xi]
     = SUM[i=1 to n] 1/i
     = ln(n) + O(1)
```

As a conclusion, even though we interview n people, we actually hire only approximately `ln(n)` of them, __on average__.
So the algorithm HIRE-ASSISTANT has an __average-case__ total hiring cost of `O(Cln(n))`. The average-case hiring cost is a significant improvement over the worst-case hiring cost of `O(Cn)`





