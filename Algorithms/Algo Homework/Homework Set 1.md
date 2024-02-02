1006954 Joshua John Lee Shi Kai
# Question 1
## $f(n) = logn$
$$
\begin{gather}
klog a = t\\
k = \frac{t}{loga} \\
f(n) \leq T \\
\frac{tlogA}{loga} \leq T \\
logA = \frac{Tloga}{t} \\
logA = loga^{T/t} \\
\Rightarrow A = \lfloor a^{\frac{T}{t}} \rfloor \\
\end{gather}
$$
## $f(n) = n^m$
$$
\begin{gather}
ka^m = t \\
k = \frac{t}{a^m} \\

f(n) \leq T \\
kA^m \leq T \\
n \leq \sqrt[m]{T/k} \\
n \leq \sqrt[m]{\frac{a^m\cdot T}{t}} \\
\Rightarrow A = \lfloor a\cdot\sqrt[m]{T/t} \rfloor
\end{gather}
$$
## $f(n) = m^n$
$$
\begin{gather}
km^a = t \\
k = \frac{t}{m^a} \\

f(n) \leq T \\
km^A \leq T \\
m^A \leq \frac{T}{k} \\
Alogm \leq log\frac{T}{k} \\
A \leq \frac{logT/k}{logm} \\
A \leq log_m\frac{T}{k} \\
A = log_m\frac{Tm^a}{t} \\
\Rightarrow A = \lfloor log_m\frac{T}{t} + a \rfloor
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
5nlog3 + 3nlog5 + 2nloge + 8nlogn \\ \leq 8nlog3 + 8nlog5 + 8nloge + 8nlogn= 8nlog15ne \\
\leq 8nlog15ne \leq 8nlogn^5 = 40nlogn, \forall n \geq e \\
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
\Rightarrow 0 \leq nlogn \leq f(n) \leq 40nlogn, \forall n \geq e\\
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
Show that 
$$
(d \times f(n) + a)^b = \Theta (f(n)^b))
$$
$\forall n \geq 1, f(n) = \Omega(n), \forall a \in \mathbb{R}, d > 0, b \in \mathbb{Z}^+$,
$$
\begin{gather}
(d \times f(n) + a)^b = (df(n))^b + \binom{b}{1}(df(n))^{b-1}a^1 + \cdots + a^b \\
\geq (f(n))^b \\\Rightarrow (d \times f(n) + a)^b = \Omega(f(n)^b), \forall n \geq 1 \\\\

(d \times f(n) + a)^b = (df(n))^b + \binom{b}{1}(df(n))^{b-1}a^1 + \cdots + a^b \\
\leq x\sum_{i=0}^{b}((df(n)^b)), \forall x > 1  \\
= xbd^bf(n)^b\\
= bkf(n)^b, \text{ where } k = xd^b, k > a^b \\
\Rightarrow (d \times f(n) + a)^b = O(f(n)^b), \forall n \geq 1
\end{gather}
$$
Therefore,
$$
\begin{gather}
(d \times f(n) + a)^b = \Omega(f(n)^b) \wedge (d \times f(n) + a)^b = O(f(n)^b), \forall n \geq 1 \\
\Rightarrow (d \times f(n) + a)^b = \Theta(f(n)^b)
\end{gather}
$$
# Question 4
## (i) $f(x) = \Theta(2^{2n})$
## (ii)  $f(x) = \Theta(slog n)$
## (iii) $f(x) = \Theta(n\sqrt{n})$
## (iv) $f(x) = \Theta(nlogn)$
## (v) $f(x) = \Theta(logn)$







