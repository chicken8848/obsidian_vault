# Vector Space Model
First, treat each document as a vector of its words, one coordinate per word of the English dictionary.

The dot product between D1 and D2 represent their similarity
$$D_1 \circ D_2 = \sum_{w} D_1(w) \cdot D_2(w)$$
For example:
- doc1 = "the cat"
- doc2 = "the dog"
Then,
$$
\begin{align}
& \text{"the cat"} \circ \text{"the dog"}  \\
&= 1 \cdot 1 + 1 \cdot 0 + 0 \cdot 1  \\
&= 1
\end{align}
$$
But,
$$
\begin{align}
& \text{"the the cat cat"} \circ \text{"the the dog dog"}  \\
&= 2 \cdot 2 + 2 \cdot 0 + 0 \cdot 2  \\
&= 4
\end{align}
$$
We then need to normalise it,
$$
\begin{gather}
|| D || = \sqrt{\sum_w D(w)^2} \\
[\text{"the cat"} \circ \text{"the dog"}] \div (\sqrt{1^2 + 1^2} \times \sqrt{1^2 + 1^2}) \\
= 0.5 \\
[\text{"the the cat cat"} \circ \text{"the the dog dog"}] \div (\sqrt{4 + 4} \times \sqrt{4 + 4}) \\
= 0.5 \\
\end{gather}
$$
Therefore, in general, for vector space model,
First normalise,
$$
\frac{D_1 \circ D_2}{||D_1|| \cdot ||D_2||}
$$
Then measure distance by angle between vectors:
$$
\theta (D_1, D_2) = \text{acos}(\frac{D_1 \circ D_2}{||D_1|| \cdot ||D_2||})
$$
If $\theta = 0$, documents are "identical" $D_1 = aD_2$
If $\theta = \frac{\pi}{2}$, they do not share a single word

# The Algorithm
1. Read file
2. Make word list (divide file into words)
3. Count frequencies of words
4. Compute dot product
	- For every word in the document, check if it appears in the other document; if yes multiply their frequencies and add to the dot product
		- worst case time: order of $\text{no of words}(D_1) \times \text{no of words}(D_2)$
	- micro-optimization:
		- sort documents into word order (alphabetically)
		- compute inner product in time $\text{no of words}(D_1) + \text{no of words}(D_2)$
	