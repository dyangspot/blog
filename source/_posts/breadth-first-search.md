title: "breadth-first-search"
date: 2015-03-24 22:09:35
tags: Algorithm

---

- Depth-first search. Put unvisited vertices on a **stack**
- Breadth-first search. Put unvisited vertices on a **queue**

With depth-first search it is either an explicit stack (with a nonrecursive version) or the function-call stack (with a recursive version).

**Shortest path**. Find path from s to t that uses **fewer number of edges**.

* * *
BFS(from source vertex s)

Put s onto a FIFO queue, and mark s as visited.
Repeat until the queue is empty:
- remove the least recently added vertex v.
- add each of v's unvisited neighbors to the queue, and mark them as visited.
* * *


**Proposition**. BFS computs shortest paths(fewest number of edges) from *s* to all other vertices in a graph in time proportional to *E + V*(# of edges + # of vertices).


### Code Implementation
``` java
public class BreadthFirstPaths{
	private boolean[] marked;
	private int[] edgeTo;
	...
	
	private void bfs(Graph G,int s){
		Queue<Integer> q = new Queue<Integer>();
		q.enqueue(s);
		marked[s] = true;
		while(!q.isEmpty()){
			int v = q.dequeue();
			for(int w :G.adj(v)){
				if(!marked[w]){
					q.enqueue[w] = true;
					edgeTo[w] = v;
				}
			}
		}
	}
}

```

###BFS application: 
- routing-> Fewest number of hops in a communication network.
- Kevin Bacon numbers;



