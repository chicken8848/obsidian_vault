# Asymptotic Complexity
## Theta $\Theta$
It means it grows asymptotically to $=$
$$n^2 = \Theta (n^2)$$
Example: 
$$0.1n^2 -100n^{1.9}+ 5 = \Theta (n^2)$$
A more precise definition:
$$ f(n) = \Theta (g(n)) \Leftrightarrow f(n) = O(g(n)) \wedge f(n) = \Omega g(n)$$
## Big O
Grows asymptotically to $\leq$
$$ n^2 = O(n^{10000}) $$
Example:
$$ 2n^3 + 100n^2 + 5 = O(n^3000) $$

A more precise definition:
$$ f(n) = O(g(n)) \Leftrightarrow \exists k > 0, n_0 \text{ such that } |f(n)| \leq k|g(n)| \text{ for some } n \geq 0$$
$k$ is called a witness
## Big Omega $\Omega$
Grows asymptotically to $\geq$
$$n^{999} = \Omega(1)$$
Example:
$$ 2n^{9999} + 100n^{33}  + 5 = \Omega(n^2) $$
A more precise definition:
$$ f(n) = \Omega (g(n)) \Leftrightarrow \exists k > 0, n_0 \text{ such that } |f(n)| \geq k|g(n)| \text{ for some } n \geq 0$$
$k$ is called a witness