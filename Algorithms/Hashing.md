# Direct address tables
Assuming we know the range of values in our list, we can store the values by their indexed by their key.
## insert
```
function insert(A,x)
	A[x.key] = x
```
Complexity: $O(1)$

## delete
```
function delete(A,x)
	A[x.key] = NIL
```
Complexity: $O(1)$

## search
```
function search(A,k)
	return A[k]
```
Complexity: $O(1)$

## Limitations
- The set of all possible key values could be very large. 
	- Many of the entries could be NIL
- The max possible key value must be known in advance
- The key values may not be integers

# Hashing
Its basically direct address tables but without any of the problems!
- no waste of memory
- prior knowledge of max possible key is not required
- key values need not be integers
## Hash functions
A function that maps arbitrary key values to a fixed set of integers, for example
$$
h(\text{Alice}) = 2
$$
A good hash function maps a very large set of key values to a small set of hash values, such that collisions are uncommon.

### Hash functions by division
Define $h$ by the map $h(k) = (k\mod m)$

### Hash functions by multiplication
Define $h$ by the map $h(k) = \lfloor m(k\beta \mod 1) \rfloor$ where $\beta > 0$, and $k\beta \mod 1$ denotes the fractional part of the number.

## Hash table
Similar to direct address table, but each object is stored based on its hash value

## Collision Resolution
What if two key values are hashed to the same slot?

### Chaining
Every slot in the hash table A is a linked list. All elements that hash to the same slot are placed in the same linked list. this allows the insertion of multiple elements with the same hash value.

Given a hash function $h$ and a new element $x$ with a key value $k$, we insert $x$ into $A$ by running `list_insert(A[h(k)],x)`. It will be inserted at the head of the linked list at `A[h(k)]`
