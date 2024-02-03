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

# Theorem
Consider $T(n) = aT(n/b) + f(n)$
$$
\begin{gather}
h = \text{ number of levels } = \log_{b}n = \Theta(\log n)\\
L = \text{ number of leaves } = a^h = a^{\log_{b}^n} = n^{\log_{b}{a}}\\
\end{gather}

$$
Let constant $\varepsilon > 0$,
if value of $f(n)$ is increasing geometrically down the tree,
$$
\begin{gather}
f(n) = O(L^{1-\varepsilon}) = O(n^{\log_b a - \varepsilon}) \\
\Rightarrow T(n) = \Theta(L) = \Theta(n^{\log_b a})
\end{gather}
$$
If the value of $f(n)$ is roughly equal at each level,
$$
\begin{gather}
f(n) = \Theta(L) = \Theta(n^{\log_b a}) \\
\Rightarrow T(n) = \Theta(L\log n) = \Theta(n^{\log_b a}) \log n)
\end{gather}
$$
If the value of $f(n)$ is decreasing down the tree,
$$
\begin{gather}
f(n) = \Omega(L^{1+\varepsilon}) = \Omega(n^{\log_b a + \varepsilon}) \\
\text{ and if } a \cdot f\left( \frac{n}{b} \right) \leq c \cdot f(n), c < 1, \forall n>n_0 \\
\Rightarrow T(n) = \Theta(f(n))
\end{gather}
$$
