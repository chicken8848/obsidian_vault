Joshua John Lee Shi Kai 1006954
# Question 1
## Part i
```
function INSERT(A,k)
	Require: A[1...n] is a max heap
	Require: k is an element to be inserted into A
	A.heap_size <- A.heap_size + 1
	A[A.heap_size] = k
	i <- A.heap_size
	while i > 1 and A[i].key > A[parent(i)].key do
		swap A[i] and A[parent(i)]
		i <- parent(i)

function INCREASE_KEY(A,i,k)
	Require: A[1...n] is a max heap
	Require: i is an integer, and 1 <= i <= n
	Require: A[i].key < k
	A[i].key <- k
	while i > 1 and A[i].key > A[parent(i)].key do
		swap A[i] and A[parent(i)]
		i <- parent(i)

function MAX_HEAPIFY(A,i)
	Require: A[1...n] is a max heap
	Require: i is an integer, and 1 <= i <= n
	if left(i) < A.heap_size and A[left(i)].key > A[i].key then
		k <- left(i)
	else
		k <- i
	if right(i) < A.heap_size and A[right(i)].key > A[k].key then
		k <- right(i)
	if k != i then
		swap A[i] and A[k]
		MAX_HEAPIFY(A,k)
```

## Part ii
```
function merge_heaps(A,B)
	Require: A array, B array
	Require: A&B be max/min heap
	C <- A.copy
	if A[0].key > A[left(0)].key:
		max = True
	else:
		max = False
	for each element k in B, do
		insert_maxmin(C,k,max)

function insert_maxmin(A,k,max)
	Require: A[1...n] is a max heap
	Require: k is an element to be inserted into A
	Require max is a boolean denoting if the heap is max or min
	A.heap_size <- A.heap_size + 1
	A[A.heap_size] = k
	i <- A.heap_size
	while i > 1 and f(A, i, max) do
		swap A[i] and A[parent(i)]
		i <- parent(i)

function f(A, i, max)
	Require: A[1...n] is a heap
	Require: i is an index in the heap
	Require: max is a boolean denoting if the heap is max or min
	if max is True:
		return A[i].key > A[parent(i)].key
	else:
		return A[i].key < A[parent(i)].key
```


# Question 2
## Part i
Pseudocode for max heapify
```
function max_heapify(A, i)
	Requires: A[1...n] array
	Requires: 0 < i < A.heapsize
	l <- left(i)
	r <- right(i)
	if l < heap_size(A) and A[l] > A[i]
		then largest = l
		else largest = i
	if r <= heap_size(A) and A[r] > A[largest]
		then largest <- r
	if largest != i
		swap(A[i], A[largest])
		max_heapify(A, largest)
```
To determine f(n), there are no loops, so we can say that it is constant $\Theta(1)$
The code determines which of the three, parent, left child, or right child is the largest, and then calls max_heapify on one of them. Since it is only one recursion call in f(n), we can say that $a = 1$.
Due to the tree-like nature of the heap, we can say that each level of recursion decreases $i$ by half, $\lfloor i / 2 \rfloor$ , as such, b = 2.
Hence the recurrence relation is as such, 
$$
\begin{gather}
T(n) = T\left( \frac{n}{2} \right) + \Theta(1) \\
\Rightarrow n^{\log_2 1} = n^0 \\
f(n) = n^0 = n^{\log_2 1} \\
\Rightarrow T(n) = \Theta(n^0 \log n) = \Theta(\log n)
\end{gather}
$$
## Part ii
If heap size is odd, then we know that the first element would be on a different node from the second element. However, they will only be one node away, meaning that they will merge at the grandparent of the two starting locations. Before that point, the differences in starting location does matter, as it might stop at one parent, and the starting locations would matter.
## Part iii
It is false. 
![[Pasted image 20240221015828.png]]
Here we note that there exists a $b$ and $d$ such that $\frac{d}{2} < b < 2d$, such that after changing the operation execution, it does not produce the same heap.
# Question 3
```
function TRI_INSERT(T,x)
	Require: T[1...n] is a tri-branch tree
	Require: x is an element to be inserted into T
	y <- NIL
	z <- T.root
	while z != NULL
		y <- z
		if x.key < z.key 
			z <- z.left
		else:
			z <- z.right
	x.parent = y
	if T.length = 0, then:
		S[0] = x
	else if x.key < y.key, then:
		y.left = x
	else:
		y.right = x

function TRI_MAX(x)
	Require: x is a node of a tri-branch tree
	while x.right != NULL:
		x = x.right
	return x

function TRI_MIN(x)
	Require: x is a node of a tri-branch tree
	while x.left != NULL:
		x = x.left
	return x

function TRI_MIDDLE(x)
	Require: x is a node of a tri-branch tree
	while x.middle != NULL
		x = x.middle
	return x

function TRI_SEARCH(x,k)
	Require: x is a node of a tri-branch tree
	Require: numerical value k
	while x.key != k and x != NULL
		if x.key > k
			x = x.left
		else
			x = x.right
	return x

function TRI_CHECK(x,k)
	Require: x is a node of a tri-branch tree
	Require: numerical value k
	counter = 0
	x = TRI_SEARCH(x,k)
	while x != NULL
		i + 1
		x = x.middle
	return counter

function TRI_SUCCESSOR(x)
	Require: x is a node of a tri-branch tree
	if x.right != NULL then:
		return TRI_MIN(x.right)
	y = x.parent
	while y != NULL and x = y.right:
		x = y
		y = y.parent
	return y
```

# Question 4
```
class Student
	attribute name
	attribute ID
	attribute score
	attribute parent
	attribute left
	attribute right
	attribute height
	attribute balance_factor = right.height - left.height

function left_rotate(x, z)
	Require: x is a student to be rotated left
	Require: z is a student right child of x and is right heavy
	inner_child = z.left
	x.right = inner_child
	if (inner_child != null)
		inner_child.parent = x
	z.left = x
	x.parent = z
	if z.balance_factor is 0
		x.balance_factor = 1
		z.balance_factor = -1
	else
		x.balance_factor = 0
		z.balance_factor = 0
	return z

function right_rotate(x, z)
	Require: x is a student to be rotated left
	Require: z is a student left child of x and is left heavy
	inner_child = z.right
	x.left = inner_child
	if (inner_child != null)
		inner_child.parent = x
	z.right = x
	x.parent = z
	if z.balance_factor is 0
		x.balance_factor = 1
		z.balance_factor = -1
	else
		x.balance_factor = 0
		z.balance_factor = 0
	return z

function right_left_rotate(x, z)
	Require: x is a student to be rebalanced
	Require: z is a right child of its parent x and balance_factor < 0
	inner_child = z.left 
	weight = inner_child.right
	z.left = weight
	if (weight != NULL) 
		weight.parent = z
	inner_child.right = z
	z.parent = inner_child
	weight2 = inner_child.left
	x.right = weight2
	if (weight != NULL)
		weight.parent = x
	inner_child.left = x
	x.parent = inner_child
	if inner_child.balance_factor is 0
		x.balance_factor = 0
		z.balance_factor = 0
	else if inner_child.balance_factor > 0
		x.balance_factor = -1
		z.balance_factor = 0
	else
		x.balance_factor = 0
		z.balance_factor = 1
	inner_child.balance_factor = 0
	return inner_child

function left_right_rotate(x, z)
	Require: x is a student to be rebalanced
	Require: z is a left child of its parent x and balance_factor > 0
	inner_child = z.right
	weight = inner_child.left
	z.right = weight
	if (weight != NULL) 
		weight.parent = z
	inner_child.left = z
	z.parent = inner_child
	weight2 = inner_child.right
	x.left = weight2
	if (weight != NULL)
		weight.parent = x
	inner_child.right = x
	x.parent = inner_child
	if inner_child.balance_factor is 0
		x.balance_factor = 0
		z.balance_factor = 0
	else if inner_child.balance_factor > 0
		x.balance_factor = -1
		z.balance_factor = 0
	else
		x.balance_factor = 0
		z.balance_factor = 1
	inner_child.balance_factor = 0
	return inner_child

function insert_student(S,x)
	Require: S is an AVL Tree
	Require: x is a student to be inserted
	y = NULL
	z = S.root
	if z is NULL:
		S.root = x
	while z != NULL
		y = z
		if x.score < z.score
			z = z.left
		else 
			z = z.right
	if x.score < y.score
		y.left = x
	else
		y.right = x

function balance_tree(S,x)
	Require: S is an AVL Tree
	Require: x the student to start from to rebalance
	z = x.parent
	while z != NULL
		if x == z.right
			if z.balance_factor > 0
				r_parent = z.parent
				if x.balance_factor < 0
					dr_node = right_left_rotate(z, x)
				else
					dr_node = left_rotate(z, x)
			else
				if z.balance_factor < 0
					z.balance_factor = 0
					break
				z.balance_factor = 1
				x = z
				continue
		else
			if z.balance_factor < 0
				r_parent = z.parent
				if x.balance_factor > 0
					dr_node = left_right_rotate(z, x)
				else
					dr_node = right_rotate(z, x)
			else
				if z.balance_factor > 0
					z.balance_factor = 0
					break
				z.balance_factor = -1
				x = z
				continue
		dr_node.parent = r_parent
		if r_parent != NULL
			if z == r_parent.left
				r_parent.left = dr_node
			else
				r_parent.right = dr_node
		else
			S.root = dr_node
			break
		z = x.parent

function NEW_STUDENT_POINTS(S,x)
	Require: S is an AVL Tree
	Require: x is a student to be inserted
	insert_student(S,x)
	balance_tree(S,x)
	

function UPDATE_POINTS(S,x,a)
	Require: S is an AVL Tree
	Require: x is a student to update the score
	Require: a is the score to update to
	x.score = a
	while x.score > x.parent.score and x.parent != NULL
		y = x.parent
		x.parent = y.parent
		y.left = x.left
		x.left = y
		temp = y.right
		y.right = x.right
		x.right = temp
	while x.score > x.right.score and y != NULL
		y = x.right
		temp = y.left
		y.left = x.left
		x.left = temp
		x.right = y.right
		y.right = x
		y.parent = x.parent
		x.parent = y
	balance_tree(S,x)

function GET_LOWEST_TEN(S)
	Require: S is an AVL tree
	if S.length < 10:
		return S
	else
		array = postorder(S.root)
		return array[0:10] // first ten nodes

function postorder(node):
	array = []
	if node = NULL, then:
		return
	array += postorder(node.left)
	array += postorder(node.right)
	array += node
	return array
```


# Question 5
![[Pasted image 20240218155657.png]]
![[Pasted image 20240221014542.png]]
