# Question 1
## Part i
$$
\begin{gather}
T(n) = \sqrt{n} T\left( \frac{n}{2} \right) \\
\end{gather}
$$
By definition, $a$ is not a constant, and therefore we cannot use the master's theorem.

As $f(n) = 0$, all nodes in the tree will take 0 time, as such $T(n) = 0$.
## Part ii
$$
\begin{gather}
T(n) = 4T\left( \frac{n}{2} \right) + n^2\log n \\
n^{\log_2 4} = n^2 \\
f(n) = n^2 \log n = \Omega(n^2) \\
\Rightarrow \log n = \Omega (n^0)
\end{gather}
$$
Which is only true when $\varepsilon = 0$. If we were to use master's theorem, $\varepsilon > 0$, then $\log n \neq \Omega(n^\varepsilon)$, hence, we cannot use master's theorem.

Going down the tree,
$$
\begin{gather}
T_{root}(n) = n^2 \log n \\
T_1(n) = 4T\left( \frac{n}{2} \right) = 4\left( \frac{n^2}{4} \right)\log \frac{n}{2} \\
\vdots \\
T_i (n) = 4^i T\left( \frac{n}{2^i} \right) = n^2\log \frac{n}{2^i} = n^2 \log n - n^2\log 2^i \\
i = \log n \\
2^i = 2^{\log n }\\
T_i(n) = n^2\log n (1 - \log 2)  \\
T(n) = \sum_{i = 0}^{\log n} 4^i T_i\left( \frac{n}{2^i} \right) \\
\Rightarrow T(n) = \log n \cdot n^2\log n (1 - \log 2) = \Theta(n^2(\log n)^2)
\end{gather}$$
Since $0 < c_1 < (1 - \log 2), c_2 > (1 - \log 2), \forall n > 0$ 



# Question 2

![[2024-02-07T02:04:24,585551679+08:00.png]]![[2024-02-07T02:05:05,402735727+08:00.png]]