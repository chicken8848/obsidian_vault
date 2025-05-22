# Homework Set 4 1006954 Joshua John Lee Shi Kai

## Question 1
```python
def question_1(G):
  if (!(G.V && G.E && G.phi)):
    raise Exception("Graph requires G.V, G.E, G.phi")
  inc = {x: [] for x in G.V}
  is_directed_graph = isinstance(G.E[0], tuple)
  for edge in G.E:
    vhead, vtail = G.phi(edge)
    if (!is_directed_graph) {
      inc[vhead].append(edge)
    }
    inc[vtail].append(edge)
  G.E = inc
  return 0
```

## Question 2
```python
def underlying_undirected_graph(G):
  if (!(G.V && G.E && G.Adj)):
    raise Exception("Graph requires G.V, G.E, G.phi")
  new_edge_set = []
  new_adjency_list = [i: [] for i in range(len(G.V))]
  for edge in G.E:
    if (edge[0] == edge[1]):
      raise Exception("u and v should be distinct")
    new_edge_set.append({edge[0], edge[1]})
    new_adjency_list[edge[0]].append(edge[1])
    new_adjency_list[edge[1]].append(edge[0])
  G.E = new_edge_set
  G.Adj = new_adjency_list
  return 0
```

## Question 3
### i
Since the input to Fleury's algorithm is that an Eulerian path exists. Then it has the constraints of an Eulerian path. Such that two bridges cannot be connected to a single vertex, by definition of a bridge, once a bridge is crossed, it cannot come back to the same component. Therefore, for any vertex `x`, it will clear all the paths that are non-bridges before crossing the path. After clearing each path, the edge is deleted, then when `x` has no other choice but to cross the bridge, it will have `deg(x) = 1`.
### ii
A simple counter-example would be the directed graph: v1 -> v2 -> v3
This graph is not strongly connected but it has a eulerian path. The example is not strongly connected as there is no path from v3 -> v1, but there is an eulerian path simply by following the path `{v1, v2, v3}`
### iii
![[Pasted image 20250307000809.png]]
### iv
John is right about (a) because for a directed graph, all vertices except the start and the end must be visited `x` times to clear `x` amount of out-degrees. Which would imply that it would have `x` amount of in-degrees. This is analog to Euler's Theorem that G has an Eulerian cycle if and only if every vertex $v \in V(G)$ has an even degree. This is because one degree is used up when approaching $v$ and another used up when leaving $v$.

John is right about (b) because of the condition that there is an Eulerian path if and only if there are either zero or two vertices $v \in V(G)$ with odd degrees.

John then puts down the condition that would either fulfil
1. last two vertices have an even degree -> the difference between in degree and out degree = 0
2. And the last two conditions are covering if they have odd degrees, up to a difference if you are choosing $u$ as the start vertex or $v$ as the starting vertex, which would change the sign.
These conditions therefore must be necessarily satisfied as they are conditions taken from Euler's Theorem for the directed graph.