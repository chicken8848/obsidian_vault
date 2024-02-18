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
If heap size is odd, then we know that the first element would be on a different node from the second element. However, they will only be one node away, meaning that they will merge at the grandparent of the two starting locations. Before that point, the differences in starting location does matter, as it might stop at one parent, and the starting locations would matter. However, past the grandparent, we would get the same heap regardless of the starting position.

## Part iii
It is true. When updating index $i$ with any value at any time, we can assume that the path taken is the same. As the path is location dependent and not key dependent. The pseudocode for swapping is as follows:
```
while i > 1 and A[i].key > A[parent(i)].key do
		swap A[i] and A[parent(i)]
		i <- parent(i)
```
As such, we only need to consider a single line from the leaf to the root node, that satisfies the max heap property, and there is only one permutation for any unique set of numbers to satisfy the max heap property. Hence it will always give the same heap.

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

function left(i)
	Require: i is an index
	return 2 * i + 1

function right(i)
	Require i is an index
	return 2 * i + 2

function parent(i)
	Require: i is an index
	return floor((i-1) / 2)
	

function NEW_STUDENT_POINTS(S,x)
	Require: S[1...n] is a max heap
	Require: x is a student to be insterted
	S.length <- S.length + 1
	S[S.heap_size] = x
	i <- S.heap_size
	while i > 1 and S[i].key > S[parent(i)].key do
		swap S[i] and S[parent(i)]
		i <- parent(i)

function UPDATE_POINTS(S,x,a)
	i = 0
	while x.key != S[i].key and S[i] != NULL
		if x.key > S[i].key
			i = right(i)
		else 
			i = left(i)
	if S[i] != NULL
		S[i].key = a
		while i > 1 and S[i].key > S[parent(i)].key do
			swap S[i] and S[parent(i)]
			i <- parent(i)

function GET_LOWEST_TEN(S)
	if S.length < 10:
		return S
	else
		heap_sort(S)
		return S[0:10]

function heap_sort(arr)
    heap_size = arr.size
    build_max_heap(arr)
    i <- arr.size
    while i >= 0 {
      swap(arr[0], arr[i]);
      max_heapify(0, i);
      i = i - 1

function build_max_heap(arr)
	i = floor(arr.size / 2)
    while i >= 0 {
      max_heapify(arr, j, arr.size())
      i = i - 1

function max_heapify(arr, index, heap_size)
	largest = index;
    l = left(index);
    r = right(index);
    if (l < heap_size and arr[l] > arr[largest]) {
      largest = l;
    }
    if (r < heap_size and arr[r] > arr[largest]) {
      largest = r;
    }
    if (largest != index) 
      swap(arr[index], arr[largest])
      max_heapify(largest, heap_size)
```


# Question 5
![[Pasted image 20240218155657.png]]
![[Pasted image 20240218155708.png]]
