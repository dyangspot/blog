title: "connected-components-dfs"
date: 2015-03-24 22:09:50
tags: Algorithm

---


Def. Vertices *v* and *w* are connected if there is a path between them.

Goal. Proepocess graph to answer queries of the form is *v* connected to *w* in **constant** time?


``` java

public class CC
	CC(Graph G)	 	// 	 find connected components in G
	boolean connected(int v,int w)        //are v and w connected?
	int count() 					//number of connected components
	int id(int v)			// component identifier for v		

```

The relation "is connected to" is an *equivalence relation*:

- Relflexive: *v* is connected to *v*.

- Symmetric: if *v* is connected to *w*,then *w* is connected to *v*.

- Transitive: if *v* connected to *w* and *w* connected to *x*, then *v* connected to *x*.

Def. A ** connected component** is a maximal set of connected vertices.

Goal. Partition vertices into connected components.


```
Initizlize all vertices *v* as unmarked.
For each unmarked vertex *v*, run DFS to identify all vertices discovered as part of the same component.
``` 
### Finding connected components with DFS

``` java
public class CC{
	private boolan marked[];
	private int[] id; // id[v] = id of component containing v
	private int count; // number of components
	
	public CC(Graph G){
		marked = new boolean[G.V()]; // create marked array
		id = new int[G.V()];		// create id array
		for(int v=0;v<G.V();v++){  // go through every vertices in graph
			if(!marked[v]){ // if v not marked 
				dfs(G,v);   //
				count++;
			}
		}
	}
	
	public int count(){		// # of components
		return count;
	}
	
	public int id(int v){ // id of component containing v
		return id[v];
	}
	
	private void dfs(Graph G,int v){
		marked[v] = true;
		id[v] = count;			// all vertices discovered in same call of dfs have same id
		for(int w:G.adj(v)){
			if(!marked[w]){
				dfs(G,w);
			}
		}
	}
}

```

### Application: particle detection
Particle detection. Give grayscale image of particles, identify "blobs."

- Vertex:pixel
- Edge: between two adjacent pixels with grayscale value >= 70
- Blob: connected component of 20-30 pixels.