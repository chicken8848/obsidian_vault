# Text Justification
## Thought Process
Define the cost of each solution as
$$
\text{cost} = \sum_\text{all lines} (\text{number of extra space characters in line})^2
$$
Let `length(i,j)` be the number of characters needed to write word $i$ to word $j-1$
Using `length(i,j)`, define a function `line_cost(i,j)` to be the number of extra characters needed to fit word $i$ to word $j-1$ into a single line with the given width $w$
$$
\text{line\_cost(i, j)} = \begin{cases}  \infty, & \text{length(i, j)} > w\\
(w - \text{length(i, j)})^2, & \text{otherwise}
\end{cases}
$$


# Longest Common Subsequence
## Thought Process
Consider the sequences
$$
\begin{gather}
X = (x_1, \dots, x_m) \\
Y = (y_1, \dots, y_n)
\end{gather}
$$
and suppose Z is the LCS of X and Y. Then at least one of the following three cases must hold.
1. $x_m = y_n$
2. $x_m \neq y_n$ and $z_k \neq x_m$
3. $x_m \neq y_n$ and $z_k \neq y_n$
If $x_m = y_n$, then $z_k = x_m = y_n$ and $z_{k-1}$ is an LCS of $X_{m-1}, Y_{n-1}$
If $x_m \neq y_n$ and $z_k \neq x_m$, then $Z$ is an LCS of $X_{m-1}, Y$
If $x_m \neq y_n$ and $z_k \neq y_n$, then $Z$ is an LCS of $X, Y_{n-1}$

## Finding Recurrence
Let `DP[i,j]` denote any LCS of $X_i, Y_j$
If $x_i = y_j$, then appending `DP[i-1, j-1]` by $x_i$ gives an LCS of $X_i$ and $Y_j$, so we can set `DP[i,j] = DP[i-1, j-1].append(x_i)`
If $x_i \neq y_j$, then either `DP[i-1, j]` or `DP[i, j-1]` would be an LCS of $X_i, Y_j$, so  we can compare the lengths of both and set `DP[i,j]` to be whichever subsequence is longer.

## The solution
### Top down
```
global memo = []
string x
string y
def DP(i,j)
	if (i,j) in memo then:
		return memo[(i,j)]
	if i = 0 or j = 0 then:
		memo[(i,j)] = []
		return []
	if x[i] = y[j] then:
		memo[(i,j)] = DP(i-1,j-1).append(x[i])
		return DP(i-1, j-1).append(x[i])
	else:
		if DP(i-1,j).length >= DP(i,j-1) then:
			memo[(i,j)] = DP(i-1,j)
			return DP(i-1,j)
		else:
			memo[(i,j)] = DP(i,j-1)
			return DP(i,j-1)
```
### Bottom Up
```
def DP_bot(i,j)
	array = [] // 2D array
	for i from 0 to m:
		array[i,0] = []
	for j from 0 to n:
		array[0,j] = []
	for i from 1 to m:
		for j from 1 to n:
			if x[i] = y[i] then:
				array[i,j] = array[i-1,j-1].append(x[i])
			else:
				if array[i-1,j].length >= array[i,j-1].length then: 
					array[i,j] = array[i-1, j]
				else:
					array[i,j] = DP[i,j-1]
	return array
```