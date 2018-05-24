
# Representing Graphs 

There are serveral ways to represent graphs with each has its advatages and disadvantages.


![alt text](http://web.cecs.pdx.edu/~sheard/course/Cs163/Graphics/graph1.png "Graph")

## Adjacency Matrix 

It is the most common implementation. Each node is associated with a integer, which is the array index. We will represent it with a two dimensional array, *array[i][j] ==1*  if there is an edge from the node with index i to the node with index j or a zero otherwise

Cons

* A lot of wasted space when representing a not strongly connected graph
* number of verices in the graph may change, and two-dimensional array is inflexible

Lets use the graph above as an example. Well there are 6 vertices so our graph is going to be 6x6 

```javascript
    const adjM = []
    /*
    * ok first so 1 is only connected to 5, 2
    * Keep in mind we are using 0 index as 1 
    */
    adjM[0] = [0, 1, 0, 0, 1, 0 ]
    // 1, 3, 5
    adjM[1] = [1, 0, 1, 0, 1, 0]
    adj[3] = [0, 1, 0, 1, 0, 0]
    adj[4] = [0, 0, 1, 0, 1, 1]
    adj[5] = [1, 1, 0, 1, 0, 0]
    adj[5] = [0, 0, 0, 1, 0, 0]
 /* 
     1  2  3  4  5  6
  1  [0, 1, 0, 0, 1, 0 ]
  2  [1, 0, 1, 0, 1, 0]
  3  [0, 1, 0, 1, 0, 0]
  4  [0, 0, 1, 0, 1, 1]
  5  [1, 1, 0, 1, 0, 0]
  6  [0, 0, 0, 1, 0, 0]
 */
```

## Adjacency List 

This consists of a list of adjacent vertices for every vertex of the graph. Different ways we can represet a list: an array, linked list, hash map or dictionary. 

```javascript
    // 
    const vertices = [1, 2, 3, 4, 5, 6];
    /* for adjList a dictionary would be easier but
    *   But im going to use a plain object so you can see whats going on
    **/
    // var adjList = new Dictionary()
    var adjList = {};

    /* this method would be in your graph class We are going to add vertices
    *   Vertices are the node or circular things
    *   Add all vertices before adding all the EDGES
    **/
    addVertex = (v) => {
        adjList[v] = [];
    }

    /*
    *   When adding the edges make sure you DONT duplicate adding the edges
    */
    addEdge = (v, w) => {
        adjList[w].push(v);
        adjList[v].push(w);
    }

    for(let i=0; i < vertices.length; i++) {
        addVertex(vertices[i]);
    }

    addEdge(1, 2)
    addEdge(1, 5)
    addEdge(2, 5)
    addEdge(2, 3)
    addEdge(3, 4)
    addEdge(4, 5)
    addEdge(4, 6)

```



## Incidence Matrix 

In an incidence matrix, each row of the matrix represents a vertex, and each column represents an edge. 

6 nodes and 6 edges 
```javascript
    // 6 nodes long 7 edges down
    const incM = []
    // 1 - 2
    incM[0] = [1, 1, 0, 0, 0, 0]
    // 1 -5
    incM[1] = [1, 0, 0, 0, 1, 0]
    // 2 -3
    incM[2] = [0, 1, 1, 0, 0, 0 ]
    // 2 - 5
    incM[3] = [0, 1, 0, 0, 1, 0]
    // 3 - 4
    incM[4] = [0, 0, 1, 1, 0, 0 ]
    // 4-5
    incM[5] = [0, 0, 0, 1, 1, 0]
    // 4 - 6
    incM[6] = [0, 0, 0, 1, 0, 1]


```