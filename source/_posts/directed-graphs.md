title: "Directed Graphs"
date: 2015-03-30 22:04:02
tags: Algorithm
---

Directed Graphs
===

Digraph. Set of vertices connected pairwise by **directed** edges.

Application: 

- Road network. Vertex = intersectiion; edge = one-way street

- Political blogosphere graph. Vertex=political blog;edge=link

- Overnight interbank loan graph. Vertex=bank; edge= overnigh load

- Combinational circuit. Vertex=logical gate;edge=wire

- WordNet graph. Vertex=synset;edge=hypernym relationship

## Digraph API

```
public class Digraph
____________________________
		Diagraph(int V)	 // create an empty diagraph with V vertices
		Diagraph(In in)    // create a diagraph from input stream
		void addEdge(int v,int w) // add a directed edge v->w
Iterable<Integer> adj(int v)	// vertices pointing from w
		 int V()					// # of vertices
		 int E()					// # of edges
	Digraph  reverse()				// reverse of this digraph
	String   toString()				// string representation
	
	
	In in = new In(args[0]);
	Diagraph G = new Diagraph(in);   // read diagraph from input stream
	
	for(int v=0;v<G.V();v++)
		for(int w:G.adj(v))
			StdOut.println(v + "->" + w);	// print out each edge(once) 			 
```

Adjacency-lists graph representation: Java implementation

```java
public class Graph Digraph{
	private final int V;
	private final Bag<Integer>[] adj;		// adjacency lists
	
	public Digraph(int V){		// create empty diagraph with v vertices
		this.V = V;
		adj=(Bag<Integer>[]) new Bag[V];
		for(int v=0;v<V;v++)
			adj[v]=new Bag<Integer>();
	}	
	
	public void addEdge(int v,int w){   // add edge v->w
		adj[v].add[w]
	}
	
	public Iterable<Integer> adj<int v> { // iterator for vertices pointing from v
		return adj[v];		
	}
}
```

## Digraph Search

### DFS in directed graphs

``` java
public class DirectedDFS{
	private boolean [] marked;   // true if from s 
	
	public DirectedDFS)(Digraph G,int s){ // constructor marks vertices reachable from s
		marked = new boolean[G.V()];
		dfs[(G,s);
	}
	
	private void dfs(Digraph G,int v){ // recursive DFS does the work
		marked[v] = true;
		for(int w:G.adj(v))
			if(!marked[w]) dfs(G,w)			
	}
	
	public boolean visited(int v){ // client can ask whether any vertex is reachable from s
		return marked[v];
	}
}
```

#### Depth-first search in digraphs summary 

DFS enables direct solution of simple digraph problems.
- Reachability.
- Path finding.
- Topological sort.
- Directed cycle detection.
Basis for solving difficult digraph problems
- 2-satisfiability.
- Directed Euler path.
- Strongly-connected components.

### BFS in digraphs

Proposition. BFS computes shortest pahts(fewest # of edges) from *s* to all other vertices n a digraph in time proportional to *E+V* 

