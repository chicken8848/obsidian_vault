# Question 1
## Part i
Yes it is possible, consider a grid of squares with only negative integers in the squares. Then any sum of the integers would be negative. However, if we pick and invalid set, the game score = 0. Which is more than a negative number.
## Part ii
In order for us to have the set be valid we extract the positive numbers

| 1   | 3   | 7   |
| --- | --- | --- |
| 4   |     | 1   |
or
0 (For choosing an invalid subset)

In which the highest score between two numbers that are not adjacent is 11.

## Part iii
Our maximum score is 16. we take the subset that gives us the maximum while not including any adjacent squares. 

## Part iv
We initialise `memo` in order to keep track of the sub-problems already solved, it stores `v(i,j)` as a 2D array.

The base case for this recurrence is `max(v(i,1),0)`

Find a maximum of $\sum_{i,j}v(i,j)$ for all $1 \leq i \leq 2, 1 \leq j \leq n$ 

```
memo = {i:{},j:{}} 
function q1(L, i, j, v)
	Require: i, j is the location of the square
	Require: i is a boolean represented by an integer
	Require: L is a 2xn grid of squares with L(i,j) representing the integer
	if memo[i][j] exists then:
		return memo[i][j]
	else if j == 1:
		memo[i][j] = L(i,j)
		return memo[i][j]
	else:
		memo[i][j] = max(q1(L, !i, j-1) + L(i,j), q1(L, i, j-1), 0)
		return memo[i][j]

return max(q1(L, 1, j, v), q1(L, 2, j, v))
```
The time complexity of this algorithm is $O(n^2)$, its space complexity is $O(n)$
# Question 2
## Part i

| a   | a   | b   |
| --- | --- | --- |
| a   | a   | c   |
We cannot reduce the tiles anymore as no 2x2 tile would fit into the grid.

## Part ii

| a   | a   | c   |
| --- | --- | --- |
| a   | a   | d   |
| b   | b   | e   |
| b   | b   | f   |
Minimum possible is 6.
m = 4, n = 3
m / 4 = 2, hence we can fit 2 2x2 tiles into the length
n // 3 = 1, hence we can only fit 1 2x2 tile into the width
As such there is at most 2x1 = 2 2x2 tiles, and the rest is filled in by 1x1 tiles.

## Part iii
m = 3, n = 7
hence the most 2x2 tiles we can fit in is 1 x 3 = 3.
the rest is filled in by 1x1 tiles. 
Therefore the minimum is 21 - 3x4 + 3 = 12 tiles

## Part iv
We initialise `table` as a 2D array in order to keep track of the sub-problems already solved, it stores the min number of tiles to fit m, and n.

The base case is m = n = 1, return 1

Find the minimum number of tiles to fit in a m x n without overlapping.

```
table = {m:{},n:{}}
function q2(m, n)
	Require: m and n is an integer
	for i in n:
			table[1][i] = 1
	for i in m:
			table[i][1] = 1
	for i in m:
		for j in n:
			if table[i][j-1] == 1 && table[i-1][j] == 1 && table[i-1][j-1] == 1:
				table[i][j] = -2
			else:
				table[i][j] = 1
	return sum(table)
```

Space complexity of $O(m \times n)$. 
Time complexity of $O(m \times n)$

# Question 3

## Part i
We can at most do 2 mini games. Hence we take the two that would give us the most points under an hour. 35 + 40 = 75
## Part ii
The biggest is 151. Following the algorithm of the knapsack problem. We end up with 151.
## Part iii
Subproblem: Compute `r[i]` for all $1\leq i \leq n$ for time j, $1 \leq j \leq k$

`memo` takes note of the maximum time and reward under j

The base case is i = 0, or j = 0, in which case we return 0

```
memo = {}
function q3(i, j)
	if (i,j) in memo then:
		return memo[(i,j)]
	if i = 0 or j = 0 then:
		memo[(i,j)] = 0
		return 0
	else:
		if t[i] > j then:
			val <- q3(i-1, j)
			memo[(i,j)]
			return val
		else:
			val <- max(q3(i-1, j), r[i] + q3(i-1, j-t[i]))
			memo <- val
			return val

run q3(n, k)
```

The time complexity is $O(2^n)$ and space complexity is $O(n)$

# Question 4
## Part i
It is possible, for example, all the digits could only be of the set
$$
A = \set{0, 2, 4, 6, 8}
$$
## Part ii
The smallest possible value for $l*(A)$ is 1. If they were ordered in descending order, then no subset would satisfy the nicely-ordered property.

## Part iii
$l*(A) = 8$. Considering the first 4 comes near the end, we consider sets that run up to and after 4. Of which the set running after 4 is longer with length 8.

## Part iv
`memo` takes note of the LCS of the subarrays that we have computed

A with length m

Then initiate the longest possible nicely ordered subarray with length n

Find the LCS

Subproblem: Compute `DP[i,j]` for all $1 \leq i \leq m, 1 \leq j \leq n$

Let `DP[i,j]` denote any LCS of $X_i, Y_j$

The base case is `DP[i,j] = {}` whenever i = 0 or j = 0

```
memo = {}
longest_possible_nicely_ordered_subarray = [i % 10 for i in A.length() + 9]
function q4(i,j)
	if (i,j) in memo then:
		return memo[(i,j)]
	if i = 0 or j = 0 then:
		memo[(i,j)] = {}
		return {}
	if A[i] == longest_possible_nicely_ordered_subarray[j] then:
		seq = DP[i-1,j-1].append(A[i])
		memo[(i,j)] = seq
		return seq
	else:
		if DP(i-1,j).length >= DP(i,j-1).length then:
			memo[(i,j)] = DP(i-1, j)
			return DP(i-1, j)
		else:
			memo[(i,j)] = DP(i, j-1)
			return DP(i, j-1)

run DP(m,n)
```

The time complexity is $O(2^n)$
The space complexity is $O(2^n)$

# Question 5

## Part i
In order to reach the (5,5) square from the (1,1) square, the minimum amount of jumps is 3. We start from the end, and find the furthest square that can reach (5,5) in one jump, and continue down until we reach (1,1)

## Part ii
If there exists `A[i][j]` $\geq m - i + n - j$
And that for such an `A[i][j]`, `A[1][1]` $\geq i - 1 + j - 1$
Then we can reach the `(m,n)` square in 2 jumps.
As we only have one intermediate square, the square must give enough energy to reach (m,n) from its given position. And then be reachable from the start position.

## Part iii
