# Concatenation
## Problem
The "+" function processes the two strings word_1 and word_2 and combines them in O(n) complexity. Unfortunately, each time a new word is appended, the machine has to process every word that has been added previously.

The overall effect: $\Theta(n^2)$ complexity

## Solution
Use this instead
```python
word_list.extend(word_1)
```

Now it is $\Theta(n)$ complexity

# Dictionaries
Use dictionaries and hashmaps if you can. As they are only $O(1)$

# Sorting algorithm
If you have to use a sorting algorithm, use a good sorting algorithm
E.g. Use merge sort instead of insertion sort

# Information
Process chunks of information rather than every piece of information
Eg. words instead of chars

#optimisation 