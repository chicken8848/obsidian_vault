A binary search tree BST is a data structure whose data storage format can be visualized as a binary tree. Whether the data is stored as an array or as a linked list, every node $x$ in a BST must have the four attributes:
- x.left, x.right, x.parent, x.key
- These attributes could be NULL
A BST must satisfy the binary-search-tree property that for every node $x$ of a BST:
- if x.left != NULL, then any node y in the left subtree of x, rooted at x.left, must satisfy y.key <= x.key
- if x.right != NULL, then any node z in the right subtree of x, rooted at x.right, must satisfy x.key <= z.key
- y.key <= x.key <= z.key for any nodes y and z contained in the left subtree and right subtree of x
![[Pasted image 20240213105049.png]]
![[Pasted image 20240213105106.png]]
# Traversal

## Pre-order traversal
- Begin at the top of the root node. Traverse leftwards, and trace the "outside" of the binary tree.
- Record down every new node visited during the traverse, starting with the root node.
```
def preorder(node):
	if node = NULL, then:
		return
	print(node.key)
	preorder(node.left)
	preorder(node.right)
```

## In-order traversal
- Begin at the top of the root node. Traverse leftward, and trace the "outside" of the binary tree.
- When you visit an unrecorded node, record this node down if either it has no left child, or it has a left child that had already been visited previously
- Same as sorted order
```
def inorder(node):
	if node = NULL, then:
		return
	inorder(node.left)
	print(node.key)
	inorder(node.right)
```

## Post-order traversal
- Begin at the top of the root node. Traverse leftwards, and trace the "outside" of the binary tree
- When you visit an unrecorded node, record tihs node down if either it is a leaf, or if this is the last time that you visit it.
```
def postorder(node):
	if node = NULL, then:
		return
	postorder(node.left)
	postorder(node.right)
	print(node.key)
```

# Operations
## Tree insert
Inputs: binary search tree T, element x
Assumptions:
- x.key != null
- x.left = x.right = null
Goal to insert element x so that the BST property still holds
```
def tree_insert(x):
	y = find_parent(T, x) // element in T that is available to be parent of x
	x.parent = y
	if T.root = NULL, then:
		T.root = x
	else if x.key < y.key, then:
		y.left = x
	else:
		y.right = x
```
How to find y:
```
def find_parent(x):
	y = NULL
	z = T.root
	while z != NULL:
		y = z
		if x.key < z.key, then:
			z = z.left
		else:
			z = z.right
```

## Tree max
```
def tree_max(x):
	while x.right != NULL
		x = x.right
	return x
```

## Tree min
```
def tree_min(x):
	while x.left != NULL:
		x = x.left
	return x
```

## Tree search
```
def tree_search(x,k):
	while x != NULL and k != x.key:
		if k < x.key then:
			x = x.left
		else:
			x = x.left
	return x
```

## Successor
Return the next element in BST w.r.t in-order
```
def successor(x):
	if x.right != NULL then:
		return tree_min(x.right)
	y = x.parent
	while y != NULL and x = y.right:
		x = y
		y = y.parent
	return y
```
If x has a right child, then by the BST property, the element with the next smallest key value must be in the right subtree
If x does not have a right child, then look at its parent, there are 3 cases:
- if x has no parent then x is the root with no right child: no successor
- if x is the left child of its parent, then successor is x.parent
- if x is the right child of its parent, then find the closest ancestor y of x such that x is a descendant of y.left
## Predecessor
Return the previous element in BST w.r.t in-order
```
def predecessor(x):
	if x.left != NULL then:
		return tree_max(x.left)
	y = x.parent
	while y != NULL and x = y.left:
		x = y
		y = y.parent
	return y
```
Same thinking process as successor
## Tree delete
Consider 3 cases:
1. x is a leaf
	1. if we remove x, the BST property still holds
2. x has exactly one child
	1. if we "promote" the child of x to replace x, the BST property still holds
3. x has exactly two children
	1. promote x.right, still maintaining the BST property]b
