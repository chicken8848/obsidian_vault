Joshua John Lee Shi Kai 1006954
# Question 1
## Part i
$$
\begin{gather}
T(n) = 1024T\left( \frac{n}{2} \right)+2^n \\
n^{\log_2 1024} = n^{10} \\
f(n) = 2^n \\
f(n) = \Omega(n^{10}) \\
1024 f\left( \frac{n}{2} \right) = 1024 \cdot 2^{n/2} = 2^{n/2 + 10} \\
2^{n/2 + 10} \leq c\cdot 2^n \\
c \geq 2^{10-n/2} \\
\Rightarrow c < 1, \forall n > 20 \\
\Rightarrow T(n) = \Theta(2^n)
\end{gather}
$$
## Part ii
$$
\begin{gather}
T(n) = 9T(\frac{n}{3}) + 5n \\
n^{\log_3 9} = n^2 \\
f(n) = 5n \\
\Rightarrow f(n) = O(n^{2-\varepsilon}), 0 < \varepsilon < 1
\end{gather}
$$
This implies that the leaves dominate,
$$
\Rightarrow T(n) = \Theta(n^2)
$$
## Part iii
$$
\begin{gather}
T(n) = 2T\left( \frac{n}{2} \right) + \frac{n^3}{\log n} \\
n^{\log_2 2} = n \\
\frac{1}{\log n} > \frac{1}{n} \\
\Rightarrow f(n) = \frac{n^3}{\log n} > \frac{n^3}{n} = n^2 > n\\
\Rightarrow f(n) = \Omega(n^{1 + \varepsilon}), 0 < \varepsilon < 1\\
2f\left( \frac{n}{2} \right) = 2 \left( \frac{n}{2} \right)^3 \frac{1}{\log \frac{n}{2}} = \frac{n^3}{4\log \frac{n}{2}} \\
\Rightarrow \exists c = \frac{1}{4} < 1, \forall n > n_0 \\
\Rightarrow T(n) = \Theta(f(n)) = \Theta\left( \frac{n^{3}}{\log n} \right)
\end{gather}
$$
## Part iv
$$
\begin{gather}
T(n) = 4T\left( \frac{n}{2} \right) + n^2 \\
n^{\log_2 4} = n^2 \\
f(n) = \Theta(n^2) \\
\Rightarrow T(n) = \Theta(n^2\log n)
\end{gather}
$$
## Part v
$$
\begin{gather}
T(n) = 8T\left( \frac{n}{2} \right) + (\log n) ^{\log n} \\
n^{\log_2 8} = n^3 \\
f(n) = (\log n) ^{\log n} = n^{\log\log n}
\end{gather}
$$
Comparing the powers of $n^3$, and $n^{\log \log n}$, there does not exist a constant $\varepsilon > 0$ such that,
$$
3 + \varepsilon = \log\log n
$$
Hence, we cannot use master's theorem.
# Question 2
## Part i
`SORTIT(A)` works on the basis that any `A[i]` to the left of ```A[count]```, where $i < \text{count}$, is already sorted. It only needs to insert `A[count]` into a sorted subset of `A` by comparing `A[count]` with `A[count - 1]`. After which it continues going toward the right of `count` again.

## Part ii
```
function MYSORT(A,B)
	Require: A[1...n]
	Require: B[1...n]
	SORTIT(A)
	SORTIT(B)
	n = A.length()
	m = B.length()
	p = n + m // find resulting array size
	output[p] // init array with size k
	i = n
	j = m
	k = 0
	while i > 0 and j > 0:
		if (A[i] >= B[j]):
			output[k] = A[i]
			i = i - 1
			k = k + 1
		else:
			output[k] = B[j]
			j = j - 1
			k = k + 1
	while i > 0:
		output[k] = A[i]
		i = i - 1
		k = k + 1
	while j > 0:
		output[k] = B[j]
		j = j - 1
		k = k + 1
```
# Question 3
## Part i
```
function EXPONENTIAL(a,n)
	Require: a is a real-number
	Require: n > 0, n in Z
	if n == 0:
		return 1
	else if n is even:
		temp = EXPONENTIAL(a, n/2) 
		return temp * temp 
	else: 
		temp = EXPONENTIAL(a, floor(n/2))
		return temp * temp * a
```
Using Master's theorem,
$$
\begin{gather}
T(n) = T\left( \frac{n}{2} \right) + 1 \\
n^{\log_2 1} = 1 \\
f(n) = 1 = O(1) \\
\Rightarrow T(n) = \Theta(n^0 \log n)= \Theta(\log n)
\end{gather}
$$
## Part ii
```
function tetration(a,n) 
	Require: a is a real-number
	Require: n > 0, n in Z
	return EXPONENTIAL(a,EXPONENTIAL(n,n))
```
$$
\begin{gather}
T(n) = f(f(n)) =( f \circ f ) (n) \\
\Theta ( f \circ f ) (n) = \Theta(f(n)) \cdot \Theta(f(n)) \\
\Rightarrow T(n) = \Theta(\log n \cdot \log n )= \Theta(\log n )^2
\end{gather}
$$
# Question 4
```
function match_num(A, B, k)
	// checking edge cases, only takes O(n)
	if len(A) + len(B) == 0:
		return False
	elif len(A) == 0:
		for i in B:
			if i == k:
			return True
		return False
	elif len(B) == 0:
		for i in A:
			if i == k:
			return True
		return False
	// sort the numbers, executes in O(nlogn) time
	merge_sort(A)
	temp_A = A[:] // making copies
	temp_B = B[:]
	i = temp_A // 2
	j = 0
	result = temp_A[i] + temp_B[j]
	while j < len(B):
		while len(temp_A) != 0:
			result = temp_A[i] + temp_B[j]
			if result == k:
				return True
			else if result > k:
				temp_A = temp_A[:i]
				i = len(temp_A) // 2
			else:
				temp_A = temp_A[i+1:]
				i = len(temp_A) // 2
		j = j + 1
		temp_A = A[:]
	return False
```
Considering the edge cases, if both arrays are empty, then it immediately returns False, as such the timing for the first case is:
$$
t_1(n) = 1
$$
The next two cases is if one of the arrays are empty, then, we need only to check if one of the arrays contain $k$ with a for loop,
$$
\begin{gather}
t_2(n) = n \\
\end{gather}
$$
The next instruction is a merge sort that completes in $O(n\log n)$ time
$$
t_3(n) = O(n\log n)
$$
The inner while loop halves the array each time it loops, as such, its time is
$$
t_4(n) = O(\log n)
$$
The outer loop iterates for each element of B, so the time is
$$
t_5(n) = O(n)
$$
Hence, the entire loop is of the time
$$
\begin{align}
t_{4+5}(n) &= O(f_5 \circ f_4 )(n) \\
&= O(f_5(n)) \cdot O(f_4(n)) \\
&= O(n) \cdot O(\log n)  \\
&= O(n\log n)
\end{align}
$$
Therefore, the entire function time is
$$
\begin{align}
T(n) &= t_1(n) + t_2(n) + t_3(n) + t_{4+5}(n) \\
&\leq 1 + n + c_1n\log n + c_2 n\log n  \\
&= O(n\log n)
\end{align}
$$
