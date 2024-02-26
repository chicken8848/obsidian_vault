Joshua John Lee Shi Kai 1006954
# Question 1
## Part i
A: 65 mod 7 = 2
L: 76 mod 7 = 6
G: 71 mod 7 = 1
O: 79 mod 7 = 2
R: 82 mod 7 = 5
I: 73 mod 7 = 3
T: 84 mod 7 = 0
H: 72 mod 7 = 2
M: 77 mod 7 = 0
$$
\begin{gather}
128 \mod 7 = 2 \\
2^8 \mod 7 = 4 \\
2^7 \mod 7 = 2 \\
2^6 \mod 7 = 1 \\
\text{nth powers of 128 mod 7 form a group of } \{4,2,1\} \\
h(\text{ALGORITHM}) = (2 \cdot 4 + 6 \cdot 2 + 1 \cdot 1 + 2 \cdot 4 \\+ 5 \cdot 2 + 3 \cdot 1 + 0 + 2 \cdot 2 + 0) \mod 7  = 4 \\
h(MHTIROGLA) = (0 + 2 \cdot 2 + 0 + 3 \cdot 4 + 5 \cdot 2 +\\ 2 \cdot 1 + 1 \cdot 4 + 6 \cdot 2 + 2 \cdot 1)\mod 7 = 4\\
\end{gather}
$$
Therefore, since the hash number for both strings are the same, it would be mapped to the same hash value.
## part ii
This is not a good choice. Using the multiplication method, we only find the fractional part of the number times the constant of 0.01. But because the constant is not irrational, in practice the distribution of the hashes would be uneven. 

# Question 2
```
function SINGLY_LINKED_INSERT(L,x)
Require: L is a singly linked list
Require: x is an object with x.key value specified
	x.next = L.he
	ad
	L.head = x

function algorithm(L1, L2)
	Require: L1 is a singly linked list
	Require: L2 is a singly linked list
	m = L1.length
	n = L2.length
	L = None
	if m > n:
		k = m - n
		while k > 0:
			SINGLY_LINKED_INSERT(L, get_max(L1, n+1-k))
			k =  k - 1
	else:
		k = n - m 
		while k > 0:
			SINGLY_LINKED_INSERT(L, get_max(L2, m+1-k))
			k = k - 1
	i = min(m, n)
	while i > 0
		SINGLY_LINKED_INSERT(L, get_max(L2, k))
		SINGLY_LINKED_INSERT(L, get_max(L1,k))
		i = i - 1

function min(m, n)
	if m > n:
		return n
	else:
		return m

function get_max(L, stop)
	Require L is a singly linked list
	Require stop is an integer
	x = L.head
	biggest = x
	while x.next != NULL:
		if x > biggest:
			biggest = x
	return x
```

# Question 3
```
function descend_merge(L1, L2)
	Require: L1 is a doubly linked list sorted in ascending order
	Require: L2 is a doubly linked list sorted in ascending order
	x = L1.head
	y = L2.head
	L = None
	while x != NULL or y != NULL
		if x!=NULL and y!=NULL:
			if y.key > x.key:
				DOUBLY_LIST_INSERT(L,x)
				x = x.next
			elif x.key > y.key:
				DOUBLY_LIST_INSERT(L,y)
				y = y.next
		elif x is NULL and y != NULL:
			DOUBLY_LIST_INSERT(L, y)
		elif y is NULL and x != NULL:
			DOUBLY_LIST_INSERT(L, x)
	return L

function DOUBLY_LIST_INSERT(L,x)
Require: L is a singly linked list
Require: x is an object with x.key value specified
	x.next = L.head
	L.head.prev = x
	L.head = x
```

# Question 4
![[Pasted image 20240226154205.png]]
