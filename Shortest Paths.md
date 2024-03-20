# The triangle inequality
If (u,v) is an edge, then by the triangle inequality,
$$
\begin{gather}
\delta (s,v) \leq \delta (s,u) + \delta(u,v) \leq \delta(s,u) + w(u,v) \\ \leq u.d + w(u,v)
\end{gather}
$$

# Bellman Ford
Complexity = $\Theta (|V| \cdot |E|)$
```
function Bellman-Ford(G,w,s)
	Require: A directed graph G with vertex set G.V and the edge set G.E
	Require: A weight function w for G. A vertex s of G
	initialize_single_source(G,s)
	for i = 1 to |G.V| - 1:
		for every directed edge (u,v) in G.E
			relax(u,v,w)
	for every directed edge (u,v) in G.E
		if v.d > u.d + w(u,v)
			return False
	return True

function relax(u,v,w):
	Require: (u,v) is a dirceted edge of G
	if u.d + w(u,v) < v.d then:
		v.d <- u.d + w(u,v)
		v.pi <- u

function initialize_single_source(G,s)
	Require: G = (V,E) is a weighted directed graph
	Require: s in V is a vertex
	for every vertex v in V:
		v.d <- inf
		v.pi <- NIL
	s.d <- 0
	
```

## Draw the table
Ref week 9 slides

|      | s.pi,s.d | t.pi,t.d  | y.pi,y.d  | x.pi,x.d  | z.pi,z.d  |
| ---- | -------- | --------- | --------- | --------- | --------- |
| Init | (NIL,0)  | (NIL,inf) | (NIL,inf) | (NIL,inf) | (NIL,inf) |
| 1    | (NIL,0)  | (s,6)     | (s,7)     | (NIL,inf) | (NIL,inf) |
| 2    | (NIL,0)  | (s,6)     | (s,7)     | (y,4)     | (t,2)     |
| 3    | (NIL,0)  | (s,6)     | (s,7)     | (y,4)     | (t,2)     |
| 4    | (NIL,0)  | (s,6)     | (s,7)     | (y,4)     | (t,-2)    |
| F.C  | (NIL,0)  | (s,6)     | (s,7)     | (y,4)     | (t,-2)    |

If you do a topological sort first, you only need to run Bellman-Ford once

```
function DAG_shortest_path(G,w,s)
	Require: A weight function w for G. A vertex s of G
	Require: A directed acyclic graph
	topological_sort(G)
	initialize_single_source(G,s)
	for each vertex u (in top_sort order):
		for each vertex in G.adj[u]:
			relax(u,v,w)
```

The table would then look like this

|      | s.pi,s.d | t.pi,t.d  | y.pi,y.d  | x.pi,x.d  | z.pi,z.d  |
| ---- | -------- | --------- | --------- | --------- | --------- |
| init | (NIL,0)  | (NIL,inf) | (NIL,inf) | (NIL,inf) | (NIL,inf) |
| s    | (NIL,0)  | s,6       | s,7       | (NIL,inf) | s,2       |
| t    | (NIL,0)  | s,6       | t,6       | t,4       | s,2       |
| y    | (NIL,0)  | s,6       | t,6       | y,2       | s,2       |
| z    | (NIL,0)  | s,6       | t,6       | y,2       | s,2       |
| x    | (NIL,0)  | s,6       | t,6       | y,2       | s,2       |

# Dijkstra
```
function dijkstra(G,w,s)
	Require: A weight function w for G. A vertex s of G
	Require: A directed graph G represented as an adjacency list, where G.Adj[u] is the linked list of vertices adjacent to u
	initialize_single_source(G,s)
	S <- {}
	Q <- G.V
	while Q != {}
		u <- extract_min(Q)
		S <- S + {u}
		for each vertex v in G.adj[u]:
			relax(u,v,w)
```

## Draw the table
Ref week 9 cohort slides

|      | s.pi,s.d | t.pi,t.d  | y.pi,y.d  | x.pi,x.d  | z.pi,z.d  |
| ---- | -------- | --------- | --------- | --------- | --------- |
| init | (NIL,0)  | (NIL,inf) | (NIL,inf) | (NIL,inf) | (NIL,inf) |
| s    | (NIL,0)  | s,10      | s,5       | (NIL,inf) | (NIL,inf) |
| y    | (NIL,0)  | y,8       | s,5       | y,14      | y,7       |
| z    | (NIL,0)  | y,8       | s,5       | z,13      | y,7       |
| t    | (NIL,0)  | y,8       | s,5       | t,9       | y,7       |
| x    | (NIL,0)  | y,8       | s,5       | t,9       | y,7       |