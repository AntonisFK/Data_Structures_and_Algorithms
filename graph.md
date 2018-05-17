# Graph

A *graph* is an abstract model of a network structure. A graph is a set of nodes connected by edges.

## Creating a Graph Class 

```javascript
    function Graph() {
        // use a array to store the names of all vertices of a graph
        var vertices = [];
        // use a dictionary  will use vertex as a key and list of adjacent vertices as value. 
        var  adjList = new Dictionary();
    }
```

Next lets implement two methods: one to add a new vertex to the graph and another method to add edges between the vertices.

```javascript 

    this.addVertex = function(v) {

        vertices.push(v);
        adjList.set(v, [] )
    }
```

Now let's implement the addEdge method

```javascript
    this.addEdge = function(v, w) {
        adjList.get(v).push(w);
        adjList.get(w).push(v);
    }
```

This method receives two vertices as parameters. First we will add an edge from vertex v to vertex w by adding w to the adjacent list v.

Lets see it in action

```javascript 
    var graph = new Graph();
    var myVertices = ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I']
    //loop thru and add dem
    for(var i=0; i < myVertices.length; i++) {
        graph.addVertex(myVertices[i]);
    }
    //lets add the edges
    graph.addEdge('A', 'B');
    graph.addEdge('A', 'C');
    graph.addEdge('A', 'D');
    //... etc
```

## Graph traversals 
Heres a useful [vid](https://www.youtube.com/watch?v=bIA8HEEUxZI&t=413s) explaining the different traversals.

### Breadth-first search (BFS)
Heres a working [Example](https://stackblitz.com/edit/js-hjg2fg). Open dev console to see output.

1. Create a queue Q.
2. Mark v as visited and enqueue v into Q.
3. While Q is not empty, perform the following: 
    1.Dequeue u from Q.
    2. Mark it as visited.
    3. Enqueue all the unvisited neighbors w of u 
    4. Mark u as explored.

when marking the vertices that we have already visited, we will use three colors to reflect their status:
    
    * *White* This represents that the vertex has not been visited
    * *Grey* Vertex has been visited but not explored
    * *Black* Vertex has been completely explored. 

```javascript
    var initializeColor = function() {
        var color = [];
        for(var i=0; i<vertices.length; i++) {
            color[vertices[i]] = 'white';
        }
        return color;
    }

    this.bfs = function(v, callback) {
        var color = initializeColor();
        var queue = new Queue();
        queue.enqueue(v);

        while(!queue.isEmpty()) {
            var u = queue.dequeue();
            var neighbors = adjList.get(u);
            color[u] = 'grey';

            for(var i=0; i < neighbors.length; i++) {
                var w = neighbors[i];
                if (color[w] === 'white') {
                    color[w] = 'grey';
                    queue.enqueue(w)
                }
            }
            color[u] = 'black';
            if (callback) {
                callback(u);
            }
        }
    }
```


## Finding the shortest path using BFS

Given graph *G* and inputing a vertices. A output will give a us the shortest distance to each vertices and predecessor array which are 
used to derive a shortest path from input vertices to other vertices.

Let see the code 


```javascript
    this.BFS = function(v) {
        var color = initializeColor(),
        queue = new Queue(),
        d = [],
        pred = [];
        // add param to queue
        queue.enqueue(v);

        for(var i=0; i<vertices.length; i++) {
            /* initilizing based on size of vertices ar
            *  not that its a 2 dem array
            */
            d[vertices[i]] = 0;
            pred[vertices[i]] = null;
        }

        while(!queue.isEmpty()) {
            var u = queue.dequeue,
            // 👇🏽 remember its from the graph class
            neighbors = adjList.get(u);
            // grey reps visited but not explored
            color[u] = 'grey';

            for(var i=0; i<neighbors.length; i++) {
                var w = neighbors[i];
            // white reps not visited
                if(color[w] === 'white') {
                    color[w] = 'grey';
            // increment the distance by + 1
                    d[w] = d[u] + 1;
                    pred[w] = u;
                    queue.enqueue(w);

                }
            }
            // pointer
            color[u] = 'black'
        }
        return {
            distance: d,
            predecessors: pred
        }
    }
/*
* Example output
*   distance:  [A: 0, B: 1, C: 1, E: 2, F: 2, G: 2, H: 2, I: 3],
    predecessors: [A: null, B: "A", C: "A", D: "A", E: "B", F: "B", G: "C", H: "D", I: "E"]
*/
```


### Shortest paths algos 

**Dijstra's algorithm** which soloves the single-source shortest path problem
**Bellman-Ford** solves the single-source problem if edge weights are negative
**A serch algorithm** provides the shortest path for a single pair of vertices using heuristics to try to speed up the search.
**Floyd-Warshall algorithm** provides the shortest path for all pairs of vertices.

## Depth-first search 

>> The DFS algorithm will start traversing the graph from the first specified vertex, will follow a path until the last vertex of this path is visited, will then backtrack, and will finally follow the next path. In other words, it visits the vertices first *deeply* and then *widely*

For Each unvistited vertex *v* in graph *G*, visit the vertex *v*,

1. Mark *v* as discovered(grey).
2. For all unvisited (white) neighbors *w* of *v*, visit vertex *w* and mark *v* as explored (black)

DFS will be revursive and uses a stack to store the calls. The stack will be created recursively. COOL 🤯

Working [example](https://stackblitz.com/edit/js-ylcbt5)

```javascript
    this.dfs = function(callback) {
        var color = initializeColor();

        for(var i=0; i<vertices.length; i++) {
            if(color[vertices[i]] === 'white') {
                // call helper function
                dfsVisit(vertices[i], color, callback)
            }
        }
    }

    // lets define helper function 
    var dfsVisit = function(u, color, callback) {
        color[u] = 'grey';
        if (callback) {
            callback(u)
        }
        // get neighbors 
        var neighbors = adjList.get(u);
        for (var i=0; i < neighbors.length; i++) {
            var w = neighbors[i];
            if(color[w] === 'white') {
                dfsVisit(w, color, callback)
            }
        }
        color[u] = 'black'
    };
```



## Dijstra's algorith 

Dijkstra algorithm is a **greedy algorithm** to calculate the shortest path between a single source and all other sources, meaning we can use it to calculate the shortest path from a graph vertex to all the other vertices. 

```javascript
    var INF = Number.MAX_SAFE_INTEGER;

    this.dijkstra = function(src) {
        var dist = [];
        var visited = [];
        var length = this.graph.length;

        for (var i = 0; i < length; i++) {
            dist[i] = INF:
            visited[i] = false;
        }

        dist[src] = 0;

        for( var i= 0; i < length - 1; i++) {

            var u = minDistance(dist, visited);

            visited[u] = true;

            for(var v = 0; v < length; v++) {
                if (!visited[v] && this.graph[u][v] != 0 && dist[u] != INF && dist[u] + this.graph[u][v] < dist[v]) {
                    dist[v] = dist[u] + this.graph[u][v];
                }
            }
        }
    }
```

```javascript

    var minDistance = function(dist, visited) {
        var min = INF;
        var minIndex = -1;

        for(var v=0; v < dist.length; v++) {
            if (visited[v] === false && dist[v] <= min) {
                min = dist[v];
                minIndex = v;
            }
        }
        return minIndex;
    };
```

## The Floyed-Warshall algorithm 

>>> The Floyd-Warshall algorithm is a dynamic programming algorithm to calculate all the shortest paths on a graph.

```javascript
    this.floydWarshall = function(){
        var dist = [];
        var length = this.graph.length;
        var i, j, k;

        for(i = 0; i < length; i++ ) {
            dist[i] = [];
            for( j= 0; j < length; j++) {
                dist[i][j] = this.graph[i][j];
            }
        }

        for(i = 0; k < length; k++) {
            for(i = 0; i < length; i++) {
                for( j = 0; j < length; j++) {
                    if (dist[i][k] + dist[k][j] < dist[i][j]) {
                        dist[i][j] = dist[i][k] + dist[k][j];
                    }
                }
            }
        }
        return dist;
    }
```