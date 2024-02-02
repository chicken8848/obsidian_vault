# Question 1
![[Pasted image 20240130133943.png]]
## Draw the truth table
| A | B | C | D | Output |
| ---- | ---- | ---- | ---- | ---- |
| 0 | 0 | 0 | 0 | 1 |
| 1 | 0 | 0 | 0 | 1 |
| 1 | 1 | 0 | 0 | 1 |
| 1 | 1 | 1 | 0 | 1 |
| 1 | 1 | 1 | 1 | 0 |
| 0 | 1 | 0 | 0 | 1 |
| 0 | 1 | 1 | 0 | 1 |
| 0 | 1 | 1 | 1 | 1 |
| 0 | 0 | 1 | 0 | 1 |
| 0 | 0 | 1 | 1 | 1 |
| 0 | 0 | 0 | 1 | 1 |
| 0 | 1 | 0 | 1 | 1 |
| 1 | 0 | 0 | 1 | 1 |
| 1 | 0 | 1 | 0 | 1 |
| 1 | 0 | 1 | 1 | 0 |
| 1 | 1 | 0 | 1 | 0 |
# Find the boolean expression
$$
F = \overline{D \wedge (B \vee C) \wedge A}
$$
# Question 2
![[Pasted image 20240130135414.png]]
# Truth table
| A | B | H(A,B) |
| ---- | ---- | ---- |
| 0 | 0 | 1 |
| 0 | 1 | 1 |
| 1 | 0 | 0 |
| 1 | 1 | 1 |
# Boolean Expression
$$
\begin{align}
H(A, B) &= \neg((A \vee B) \wedge \neg((\neg(A\vee\overline{B}))\vee (A \wedge B))) \\
&= \neg((A \vee B) \wedge \neg((\neg(A\vee\overline{B}))\vee (A \wedge B)))
\end{align}
$$