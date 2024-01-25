# Question 1
- Find biggest $A$ that can be processed in $T$ seconds.
- Algorithm can process array of size $a$ in $t$ seconds.
## $f(n) = logn$
$$
\begin{gather}
f(n) \leq T \\
logn \leq T \\
n \leq e^T \\
\Rightarrow A = e^T
\end{gather}
$$
## $f(n) = n^m$
$$
\begin{gather}
f(n) \leq T \\
n^m \leq T \\
n \leq \sqrt[m]{T} \\
\Rightarrow A = \sqrt[m]{T}
\end{gather}
$$
## $f(n) = m^n$
$$
\begin{gather}
f(n) \leq T \\
n log m \leq log T \\
n \leq \frac{logT}{logm} \\
n \leq log_mT \\
\Rightarrow A = log_mT
\end{gather}
$$

# Question 2
## Part (i)
Why is 
$$
0 \leq 2n^2 \leq 3n^2 - 2n \leq 3n^2
$$
true for all $n \geq 2$?
$$
\begin{gather}
0 \leq 2n^2 \text{ for } n \geq 0 \\\\
2n^2 \leq 3n^2-2n \\
\Rightarrow 0 \leq n^2 - 2n \text{ for } n \geq 2 \\\\
3n^2 - 2n \leq 3n^2 \\
\Rightarrow -2n \leq 0 \text{ for } n \geq 0 \\\\

\Rightarrow 0 \leq 2n^2 \leq 3n^2 - 2n \leq 3n^2, \forall n \geq 2

\end{gather}
$$
## Part (ii) 
$$
\begin{gather}
f(n) = 5nlog(3n)+3nlog(5n) + 2n
\end{gather}
$$
In order to show that $f(n) = \Theta(g(n))$, we must show that
$$
\begin{gather}
\exists c_1, c_2, \forall n> n_0 \text{ such that } \\
0 \leq c_1 g(n) \leq f(n) \leq c_2g(n)
\end{gather}
$$
Rearranging some terms,
$$
\begin{align}
f(n) &= log(3n)^{5n} + log(5n)^{3n} + log(e)^{2n} \\
&= nlog((3n)^5(5n)^3(e)^2) \\
&= nlog3^55^3e^2n^8 \\
&= n(5log3 + 3log5+2loge + 8logn) \\
&= 5nlog3 + 3nlog5 + 2nloge + 8nlogn \\\\
\end{align}

$$
We first show that $f(n) = O(nlogn)$,
$$
\begin{gather}
5nlog3 + 3nlog5 + 2nloge + 8nlogn \\ \leq 5nlog3 + 5nlog5 + 5nloge = 15nlog15e \\
\leq 15nlog15n \leq 15nlogn^4 = 45nlogn, \forall n \geq e \\
\Rightarrow f(n) = O(nlogn)
\end{gather}
$$

Now we show that $f(n) = \Omega(nlogn)$,
$$
\begin{align}
f(n) &= 5nlog3 + 3nlog5 + 2nloge + 8nlogn \\
&\geq nlogn, \forall n \geq e \\
\Rightarrow & f(n) = \Omega(nlogn)
\end{align}
$$
Therefore,
$$
\begin{gather}
f(n) = \Omega(nlogn) \wedge f(n) = O(nlogn) \\
\Rightarrow 0 \leq nlogn \leq f(n) \leq 45nlogn, \forall n \geq e\\
\Rightarrow f(n) = \Theta(nlogn)
\end{gather}
$$
## Part (iii)
$$
f(n) = 2^{2n} + 3^n + 4n(2^n)
$$
In order to show that $f(n) = \Theta(g(n))$, we must show that
$$
\begin{gather}
\exists c_1, c_2, \forall n> n_0 \text{ such that } \\
0 \leq c_1 g(n) \leq f(n) \leq c_2g(n)
\end{gather}
$$
First we show that $f(n) = \Omega(2^{2n})$,
$$
\begin{align}
f(n) &= 2^{2n} + 3^n + 4n(2^n) \\
&\geq 2^{2n}, \forall n \geq 1 \\
\Rightarrow & f(n) = \Omega(2^{2n})
\end{align}
$$
Next we show that $f(n) = O(2^{2n})$,
$$
\begin{gather}
2^{2n} + 3^n + 4n(2^n) = 2^{2n}+ 3^n +n(2^{n+2}) \\
\leq 2^{2n} + 2^{2n} + 2^{2n}, \forall n \geq 4
\end{gather}
$$
Therefore,
$$
\begin{gather}
f(n) = \Omega(2^{2n}) \wedge f(n) = O(2^{2n}) \\
\Rightarrow 0 \leq 2^{2n} \leq f(n) \leq 3\cdot 2^{2n}, \forall n \geq 4\\
\Rightarrow f(n) = \Theta(2^{2n})
\end{gather}
$$

# Question 3

## Part (i)
For any non-negative functions $f(n)$ and $g(n)$, to show that $f(n) = \Theta(g(n))$, we must show that
$$
\begin{gather}
\exists c_1,c_2, \text{ such that } \\
0 \leq c_1g(n) \leq f(n) \leq c_2g(n), \forall n \geq n_0
\end{gather}
$$
This implies that,
$$
\begin{gather}
f(n) = \Omega(g(n)) \Rightarrow g(n) = O(f(n)) \\
\text{since } \exists k_1, \text{ such that } |f(n)|  \geq k_1|g(n)|, \forall n \geq n_0 \\
\text{then } |g(n)| \leq \frac{1}{k_1}|f(n)| = k_2|f(n)|, \forall n\geq n_0 \\
\end{gather}
$$
Similarly,
$$
\begin{gather}
f(n) = O(g(n)) \Rightarrow g(n) = \Omega(f(n)) \\

\text{since } \exists k_1, \text{ such that } |f(n)|  \leq k_1|g(n)|, \forall n \geq n_0 \\
\text{then } |g(n)| \geq \frac{1}{k_1}|f(n)| = k_2|f(n)|, \forall n\geq n_0 \\

\end{gather}
$$
Therefore,
$$
\begin{gather}
f(n) = \Theta(g(n)) \Rightarrow f(n) = \Omega(g(n)) \wedge f(n) = O(g(n)) \\
\Rightarrow g(n) = O(f(n)) \wedge g(n) = \Omega(f(n)) \\
\Rightarrow g(n) =\Theta(f(n))
\end{gather}
$$
Similarly,
$$
\begin{gather}
g(n) = \Theta(f(n)) \Rightarrow g(n) = \Omega(f(n)) \wedge g(n) = O(f(n)) \\
\Rightarrow f(n) = O(g(n)) \wedge f(n) = \Omega(g(n)) \\
\Rightarrow f(n) =\Theta(g(n))
\end{gather}
$$
Finally, since
$$
f(n) = \Theta(g(n)) \Rightarrow g(n) = \Theta(f(n))
$$
and
$$
g(n) = \Theta(f(n)) \Rightarrow f(n)) = \Theta(g(n))
$$
then,
$$
f(n) = \Theta(g(n)) \Leftrightarrow g(n) = \Theta(f(n))
$$

## Part (ii)





