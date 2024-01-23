# Regardless of Base
We are given that an algorithm has complexity O(log n) in the given input size n. Explain why the complexity for this algorithm is also O(log𝑏 𝑛), regardless of the choice of any base b > 1 for the logarithm appearing in the expression.

Prove that 
$$
f(n) = O(logn) \Rightarrow f(n) = O(log_bn), \forall b > 1
$$
Proof:
$$
\begin{gather}

f(n) = O(logn) \Leftrightarrow \exists c > 0, n_o \in N \text{ such that } \\
0 \leq f(n) \leq c \cdot logn, \forall n > n_0\\
\end{gather}
$$
Similarly:
$$
\begin{gather}
f(n) = O(log_bn) \Leftrightarrow \exists d > 0, n_1 \in N \text{ such that } \\
0 \leq f(n) \leq d \cdot logn, \forall n > n_1

\end{gather}
$$
Let $d = c \times logb$ and $n_1 = n_0$
So,
$$
\begin{gather}
0 \leq f(n) \leq c \cdot \frac{logb}{logb} logn \\
0 \leq f(n) \leq d \cdot \frac{logn}{logb} \\
= d \cdot log_b(n) \\
\end{gather}
$$
# Other problems
Prove that $nlogn = O(n^2)$
$$
\begin{gather}
nlogn = O(logn) \Leftrightarrow \exists c > 0, n_o \in N \text{ such that } \\
0 \leq nlogn \leq c \cdot logn, \forall n > n_o
\end{gather}
$$
We know that $\forall n \geq 1$, $logn \leq n$
Therefore,
$$
\begin{gather}
logn \leq n \\
n \cdot logn \leq n \cdot n \\
\end{gather}
$$

Prove that $n \times 2^{n-1} = \Omega(n+2^n)$
$$
\begin{gather}
n\times 2^{n-1} = \Omega(logn) \Leftrightarrow \exists c > 0, n_o \in N \text{ such that } \\
0 \leq c\cdot logn \leq n \times 2^{n-1}, \forall n > n_o
\end{gather}
$$
for $\forall n \geq 4$,
$$
\begin{gather}
n \times 2^{n-1} \geq 2^{n+1} = 2^{n} + 2^{n}\\
2^{n} + 2^{n} \geq n + 2^n, \forall n\geq4 \\
\end{gather}
$$

Prove that $\frac{n^2}{2} - 3 \times n = \Theta(n^2)$
$$
c_1 \cdot n^2 \leq \frac{n^2}{2} - 3 \cdot n \leq c_2 \cdot n^2
$$
$$
\begin{gather}
\forall n \geq 7 \\
c_1 = \frac{1}{50} \\
c_2 = 1
\end{gather}
$$

Prove that $6 \times n^3 \neq \Theta (n^2)$
$$
6 \times n^3 \geq n^3 
$$

Prove that $\sum_1^n \frac{1}{k} = \Theta(\int_1^n \frac{1}{x} dx)$
$$
\begin{gather}
\int_1^n \frac{dx}{x} \leq 1 + \frac{1}{2} + \frac{1}{3} + \cdots + \frac{1}{n-1} \leq 1 + \frac{1}{2} + \frac{1}{3} + \cdots + \frac{1}{n} \\
= \sum_1^n \frac{1}{k} \\

1 + \int_1^n \frac{dx}{x} \geq 1 + \frac{1}{2} + \frac{1}{3} + \cdots + \frac{1}{n} \\
= \sum_1^n \frac{1}{k} \\

\Rightarrow \int_1^n \frac{dx}{x} \leq \sum_1^n \frac{1}{k} \leq 1 + \int_1^n \frac{dx}{x}
\end{gather}
$$
Note that $\int_1^n \frac{dx}{x} = ln(n) \geq 1, \forall n \geq 3$
So,
$$
\begin{gather}
\int_1^n \frac{dx}{x} \leq \sum_1^n \frac{1}{k} \leq  \int_1^n \frac{dx}{x}+ \int_1^n \frac{dx}{x} \\
\int_1^n \frac{dx}{x} \leq \sum_1^n \frac{1}{k} \leq  2 \cdot \int_1^n \frac{dx}{x}
\end{gather}
$$
