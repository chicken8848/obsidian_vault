Break the problem into several sub-problems that are similar to the original problem, but smaller in size. Then solve recursively.

# Key Idea
- Divide input into parts (smaller problems)
- Conquer each part recursively
- Combine results to obtain solution of original
$$
\begin{align}
T(n) &= \text{ divide time } \\
&+ T(n_1) + T(n_2) + \cdots + T(n_k) \\
&+ \text{ combine time } \\
T(n) &= aT(n/b) + f(n) \\
\end{align}
$$
When the sub-problems are large enough to solve recursively, we call that the recursive case.

Once the sub-problems become small enough that we no longer recurse, we say that we have gotten down to the base case.

Base cases look like leaves of the tree.

A recurrence is an equation or inequality that describes a function in terms of its value on smaller inputs.

An example,

$$
T(n) = \begin{cases}
\Theta(1), n = 1 \\
2T(n/3) + \Theta(n), \forall n > 1
\end{cases}
$$
# Merge Sort
Time:
1. Divide: $\Theta(n)$
2. recursion : $T(n_1) = T(n_2) = 2T(n/2)$
3. Merge: \Theta(n)
Total: $T(n) = 2T(n/2) + \Theta(n)$

# Finding the cost
## Recursion Tree
The recursion tree = sum of cost of nodes

For merge sort:
![[Pasted image 20240129094210.png]]

So, height = $nlogn$, and at each level, cost = $n$
$$
\begin{gather}
\text{Cost } = n + 2\cdot \frac{n}{2} + \cdots + 2^k \cdot n/2^k + nlogn \\
T(n) = \Theta (nlogn)
\end{gather}
$$

Solution by expansion:

$$
\begin{align}
T(n) &= cn + c\cdot \frac{n}{2} + c\cdot \frac{n}{4} + \cdots \\
&= cn(1 + \frac{1}{2} + \frac{1}{4} + \cdots) \\
&\leq cn(2) \\
&= \Theta(n)
\end{align}
$$

## Substitution method
1. Make a guess first
2. Use substitution method to prove it
For merge sort, find some condition over $c, n_0$, such that,
$$
0 \leq T(n) \leq cnlogn \\
$$
$$
\begin{align}
T(n) &= 2T(\frac{n}{2}) + n \\
T(n) &= 2c(\frac{n}{2}log\frac{n}{2}) + n \\
&= cnlogn + n - cnlog2 \\
&= cnlogn + n(1-clog2) \\
&\leq cnlogn, \forall c \geq \frac{1}{log2} \\
T(n) &= cnlogn > 1 \\
\Rightarrow n_0 &> 1
\end{align}
$$
# Some considerations
There are a few cases to consider
- Each level's value is the same
	- $f(n) * log_b n$
	- Total value = $\Theta$(number of levels x sum of each level)
- Each level's value is decreasing 
	- $f(n)$
	- total value = $\Theta$(root node's value)
- Each level's value is increasing 
	- $n^{log_b a}$
	- total value = $\Theta$(number of leaves)



