Joshua John Lee Shi Kai 1006954
# Question 1
## Part i
$$
\begin{gather}
T(n) = 1024T(\frac{n}{2})+2^n \\
n^{\log_2 1024} = n^{10} \\
f(n) = 2^n \\
\end{gather}
$$
Comparing the powers of $n^{10}$ and $2^n$, we realise that there does not exist a $\varepsilon$ such that 
$$
10 + \varepsilon = n
$$
As such we are unable to use master's theorem
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
\Rightarrow T(n) = n^2\log n
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
Comparing the powers of $n^3$, and $n^{\log \log n}$, there does not exist a $\varepsilon$ such that,
$$
3 + \varepsilon = \log n
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
	if n = 0:
		return 1
	else:
		return EXPONENTIAL(a, floor(n/2)) * EXPONENTIAL(a, ceiling(n/2))
```
## Part ii
```
function tetration(a,n) 
	Require: a is a real-number
	Require: n > 0, n in Z
	return EXPONENTIAL(EXPONENTIAL(a,n),n)
```
$$
\begin{gather}
T(n) = f(f(n)) =( f \circ f ) (n) \\
\Theta ( f \circ f ) (n) = \Theta(f(n)) \cdot \Theta(f(n)) \\
\Rightarrow T(n) = \log n \cdot \log n = (\log n )^2
\end{gather}
$$
# Question 4