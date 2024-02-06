In order to satisfy the max-heap property:
- Leafs do not violate the max heap property
- the children of node $i$, together with all the notes below them, do not violate the max-heap property
# Building the heap
## max-heapify
```
max_heapify:
	l <- left(i)
	r <- right(i)
	if l <= heap-size(A) and A[l] > A[i]
		then largest <- l
		else largest <- i
	if r <= heap-size(A) and A[r] > A[largest]
		then largest <- r
	if largest != i
		then exchange A[i] and A[largest]
		MAX_HEAPIFY(A,largest)
```
## build_max_heap
```
build_max_heap(A):
	heap_size(A) = length(A)
	for i <- floor(length(A)/2)) down to 1
		do max_heapify(A,i)
```
# Heap-sort algorithm
1. Build a max heap from A
2. Find the maximum element A[0]
3. Swap elements A[A.heap_size-1] and A[0]
Largest element is now at the end of the array!
4. discard node n from the heap A.heapsize <- A.heapsize - 1
The new root may violate max-heap property, but its children are the roots of sub-trees that are max heaps
5. Run max_heapify(A,1) to fix this violation
6. Go to step 2
## Complexity
Input: An array A of length n
- In every iteration, A.heap_size decreases by 1
- after $n$ iterations, the heap size of A becomes 0
- Every iteration involves a swap and a max_heapify
	- Each swap has complexity O(1)
	- Each max_heapify call has complexity $O(\log n)$
Complexity is $O(n \log n)$

# Insert into heap
$O(\log n)$
1. Insert at the end
2. call max_heapify
# Extract max
$O(\log n)$
1. Remove the root node
2. replace it by last node
3. call max_heapify
# Increase key
$O(\log n)$
```
A[i] <- k
while i > 1 and A[parent(i)] < A[i]
	swap A[i] with A[parent(i)]
	i <- parent(i)
```
