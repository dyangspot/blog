title: "depth-first-search"
date: 2015-03-24 00:29:30
tags: Algorithm
---

Graph
===
## Tremaux maze exploration
* make each visited intersection and each visited passage
* Retrace steps when no unvisted options

## Depth-first search 
- Goal. Systematically search through a graph, find all vertices connected to s.
- Idea. Mimic maze exploration

```
 DFS(to visit a vertex v)
 Mark v as visited.
 Recursively visit all unmarked vertices w adjacent to v.
```

### Design pattern

- Decouple graph data type from graph processing.
- Create a Graph object.
-  Pass the Graph to graph-processing routine.
- Query the graph-processing routine for information.

### Algorithm.

- Use recursion(ball of string).
- Mark each visited vertex(and keep track of edge taken to visit to it).
- Return(retrace steps) when no unvisited options.

### Data structures.
-  **boolean[] marked** to mark visited vertices
- **int[] edgeTo** to keep tree of paths.
(edgeTo[w] == v) means that edge v-w taken to visit w for the first time.

### Java API

``` java
public class Paths

Paths(Graph G,int s) //find paths in G from source s 
boolean hasPathTo(int v) //is there a path from s to v?
Iterable<Integer> pathTo(int v) //path from s to v;null if no such path

```
### Code Implemention
``` java
Paths paths = new Paths(G,s);
for(int v = 0; v < G.V(); v++){
	if(paths.hasPathTo(v))
		StdOut.println(v);
}
```

### Depth-first search code

``` java
public class DepthFirstPaths{
	private boolean[] marked;
	private int[] edgeTo;
	private int s;

	public DepthFirstPaths(Graph G,int s){
		…
		dfs(G,s);
	}
	
	private void dfs(Graph G,int v){
		marked[v] = true;
		for(int w:G.adj(v)){
			if(!marked[w]){
				dfs(G,w);
				edgeTo[w] = v;
			}
		}
	}
}
```

![DFS used for dating](http://imgs.xkcd.com/comics/dfs.png )

