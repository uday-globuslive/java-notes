# Graph Algorithms in Java

Graphs are one of the most versatile and important data structures in computer science. They represent a collection of nodes (vertices) connected by edges and can model a wide variety of real-world problems. This guide covers the fundamental concepts of graph theory, common graph algorithms, and their implementations in Java.

## Table of Contents
- [Introduction to Graphs](#introduction-to-graphs)
- [Graph Representations](#graph-representations)
- [Graph Traversal Algorithms](#graph-traversal-algorithms)
- [Shortest Path Algorithms](#shortest-path-algorithms)
- [Minimum Spanning Tree Algorithms](#minimum-spanning-tree-algorithms)
- [Topological Sorting](#topological-sorting)
- [Strongly Connected Components](#strongly-connected-components)
- [Advanced Graph Algorithms](#advanced-graph-algorithms)
- [Common Interview Problems](#common-interview-problems)
- [Practice Problems](#practice-problems)

## Introduction to Graphs

A graph is a data structure consisting of a finite set of vertices (also called nodes) and a set of edges connecting these vertices. Graphs are used to represent networks, social connections, web pages, road maps, and many other real-world entities.

### Types of Graphs

1. **Directed vs Undirected**:
   - In a directed graph (digraph), edges have a direction and can be traversed in one direction only.
   - In an undirected graph, edges have no direction and can be traversed in both directions.

2. **Weighted vs Unweighted**:
   - In a weighted graph, each edge has a weight/cost associated with it.
   - In an unweighted graph, no weight is associated with edges.

3. **Connected vs Disconnected**:
   - A connected graph has a path between every pair of vertices.
   - A disconnected graph has at least one vertex that cannot be reached from some other vertex.

4. **Cyclic vs Acyclic**:
   - A cyclic graph contains at least one cycle.
   - An acyclic graph contains no cycles.

5. **Special Types**:
   - Tree: A connected acyclic undirected graph.
   - Directed Acyclic Graph (DAG): A directed graph with no cycles.
   - Complete Graph: A graph where every vertex is connected to every other vertex.
   - Bipartite Graph: A graph whose vertices can be divided into two disjoint sets such that every edge connects vertices from different sets.

## Graph Representations

There are several ways to represent graphs in computer memory:

### 1. Adjacency Matrix

A 2D array where matrix[i][j] = 1 (or weight) if there's an edge from vertex i to vertex j, and 0 otherwise.

```java
public class AdjacencyMatrix {
    private int V; // Number of vertices
    private int[][] matrix;
    
    public AdjacencyMatrix(int v) {
        V = v;
        matrix = new int[v][v];
    }
    
    // Add an edge (for unweighted graph)
    public void addEdge(int source, int destination) {
        matrix[source][destination] = 1;
        // For undirected graph
        // matrix[destination][source] = 1;
    }
    
    // Add weighted edge
    public void addWeightedEdge(int source, int destination, int weight) {
        matrix[source][destination] = weight;
        // For undirected graph
        // matrix[destination][source] = weight;
    }
    
    // Print the matrix
    public void printGraph() {
        for (int i = 0; i < V; i++) {
            for (int j = 0; j < V; j++) {
                System.out.print(matrix[i][j] + " ");
            }
            System.out.println();
        }
    }
}
```

### 2. Adjacency List

A collection of lists or arrays where the ith list contains vertices adjacent to the ith vertex.

```java
public class AdjacencyList {
    private int V; // Number of vertices
    private List<List<Integer>> adj; // Adjacency Lists
    
    public AdjacencyList(int v) {
        V = v;
        adj = new ArrayList<>(v);
        for (int i = 0; i < v; i++) {
            adj.add(new ArrayList<>());
        }
    }
    
    // Add an edge
    public void addEdge(int source, int destination) {
        adj.get(source).add(destination);
        // For undirected graph
        // adj.get(destination).add(source);
    }
    
    // Print the graph
    public void printGraph() {
        for (int i = 0; i < V; i++) {
            System.out.print("Vertex " + i + " is connected to: ");
            for (int j = 0; j < adj.get(i).size(); j++) {
                System.out.print(adj.get(i).get(j) + " ");
            }
            System.out.println();
        }
    }
}
```

### 3. Adjacency List for Weighted Graphs

For weighted graphs, we store pairs of (vertex, weight).

```java
public class WeightedAdjacencyList {
    private int V;
    private List<List<Pair<Integer, Integer>>> adj; // (destination, weight)
    
    public WeightedAdjacencyList(int v) {
        V = v;
        adj = new ArrayList<>(v);
        for (int i = 0; i < v; i++) {
            adj.add(new ArrayList<>());
        }
    }
    
    // Add a weighted edge
    public void addEdge(int source, int destination, int weight) {
        adj.get(source).add(new Pair<>(destination, weight));
        // For undirected graph
        // adj.get(destination).add(new Pair<>(source, weight));
    }
    
    // Print the graph
    public void printGraph() {
        for (int i = 0; i < V; i++) {
            System.out.print("Vertex " + i + " is connected to: ");
            for (Pair<Integer, Integer> edge : adj.get(i)) {
                System.out.print("(" + edge.getKey() + ", " + edge.getValue() + ") ");
            }
            System.out.println();
        }
    }
    
    static class Pair<K, V> {
        private K key;
        private V value;
        
        public Pair(K key, V value) {
            this.key = key;
            this.value = value;
        }
        
        public K getKey() {
            return key;
        }
        
        public V getValue() {
            return value;
        }
    }
}
```

### Comparison of Representations

| Operation | Adjacency Matrix | Adjacency List |
|-----------|------------------|---------------|
| Space     | O(V²)            | O(V + E)      |
| Check if edge exists | O(1) | O(V) in worst case |
| Iterate all edges | O(V²) | O(V + E) |
| Find all vertices adjacent to v | O(V) | O(degree(v)) |

- Use an adjacency matrix when the graph is dense (many edges) or when you need quick edge lookup.
- Use an adjacency list when the graph is sparse (few edges) or when you need to iterate over adjacent vertices frequently.

## Graph Traversal Algorithms

### 1. Breadth-First Search (BFS)

BFS explores all vertices at the present depth before moving on to vertices at the next depth level. It uses a queue to keep track of the next vertices to visit.

```java
public void bfs(int startVertex) {
    boolean[] visited = new boolean[V];
    Queue<Integer> queue = new LinkedList<>();
    
    visited[startVertex] = true;
    queue.add(startVertex);
    
    while (!queue.isEmpty()) {
        int vertex = queue.poll();
        System.out.print(vertex + " ");
        
        for (int adjacent : adj.get(vertex)) {
            if (!visited[adjacent]) {
                visited[adjacent] = true;
                queue.add(adjacent);
            }
        }
    }
}
```

Applications of BFS:
- Finding the shortest path in an unweighted graph
- Finding all connected components
- Testing if a graph is bipartite
- Finding the "closest" nodes in the graph

### 2. Depth-First Search (DFS)

DFS explores as far as possible along each branch before backtracking. It can be implemented recursively or using a stack.

```java
// Recursive DFS
public void dfs(int startVertex) {
    boolean[] visited = new boolean[V];
    dfsUtil(startVertex, visited);
}

private void dfsUtil(int vertex, boolean[] visited) {
    visited[vertex] = true;
    System.out.print(vertex + " ");
    
    for (int adjacent : adj.get(vertex)) {
        if (!visited[adjacent]) {
            dfsUtil(adjacent, visited);
        }
    }
}

// Iterative DFS using a stack
public void dfsIterative(int startVertex) {
    boolean[] visited = new boolean[V];
    Stack<Integer> stack = new Stack<>();
    
    stack.push(startVertex);
    
    while (!stack.isEmpty()) {
        int vertex = stack.pop();
        
        if (!visited[vertex]) {
            visited[vertex] = true;
            System.out.print(vertex + " ");
            
            // Push all unvisited adjacent vertices
            List<Integer> adjList = adj.get(vertex);
            for (int i = adjList.size() - 1; i >= 0; i--) {
                if (!visited[adjList.get(i)]) {
                    stack.push(adjList.get(i));
                }
            }
        }
    }
}
```

Applications of DFS:
- Detecting cycles in a graph
- Topological sorting
- Finding strongly connected components
- Solving puzzles (like mazes)
- Generating a maze

## Shortest Path Algorithms

### 1. Dijkstra's Algorithm

Dijkstra's algorithm finds the shortest path from a source vertex to all other vertices in a weighted graph with non-negative edge weights.

```java
public void dijkstra(int source) {
    int[] distance = new int[V];
    boolean[] visited = new boolean[V];
    
    // Initialize distances
    for (int i = 0; i < V; i++) {
        distance[i] = Integer.MAX_VALUE;
    }
    distance[source] = 0;
    
    // Find shortest path for all vertices
    for (int count = 0; count < V - 1; count++) {
        // Find the minimum distance vertex from the unprocessed set
        int u = minDistance(distance, visited);
        
        // Mark the selected vertex as processed
        visited[u] = true;
        
        // Update distance value of the adjacent vertices
        for (Pair<Integer, Integer> edge : adj.get(u)) {
            int v = edge.getKey();
            int weight = edge.getValue();
            
            if (!visited[v] && distance[u] != Integer.MAX_VALUE && 
                distance[u] + weight < distance[v]) {
                distance[v] = distance[u] + weight;
            }
        }
    }
    
    // Print the shortest distances
    printSolution(distance);
}

private int minDistance(int[] distance, boolean[] visited) {
    int min = Integer.MAX_VALUE, minIndex = -1;
    
    for (int v = 0; v < V; v++) {
        if (!visited[v] && distance[v] <= min) {
            min = distance[v];
            minIndex = v;
        }
    }
    
    return minIndex;
}

private void printSolution(int[] distance) {
    System.out.println("Vertex \t Distance from Source");
    for (int i = 0; i < V; i++) {
        System.out.println(i + " \t " + distance[i]);
    }
}
```

### 2. Bellman-Ford Algorithm

The Bellman-Ford algorithm finds shortest paths from a source vertex to all other vertices in a weighted graph. Unlike Dijkstra's, it can handle graphs with negative edge weights and can detect negative weight cycles.

```java
public void bellmanFord(int source) {
    int[] distance = new int[V];
    
    // Initialize distances
    for (int i = 0; i < V; i++) {
        distance[i] = Integer.MAX_VALUE;
    }
    distance[source] = 0;
    
    // Relax all edges V-1 times
    for (int i = 0; i < V - 1; i++) {
        for (int u = 0; u < V; u++) {
            for (Pair<Integer, Integer> edge : adj.get(u)) {
                int v = edge.getKey();
                int weight = edge.getValue();
                
                if (distance[u] != Integer.MAX_VALUE && distance[u] + weight < distance[v]) {
                    distance[v] = distance[u] + weight;
                }
            }
        }
    }
    
    // Check for negative-weight cycles
    for (int u = 0; u < V; u++) {
        for (Pair<Integer, Integer> edge : adj.get(u)) {
            int v = edge.getKey();
            int weight = edge.getValue();
            
            if (distance[u] != Integer.MAX_VALUE && distance[u] + weight < distance[v]) {
                System.out.println("Graph contains negative weight cycle");
                return;
            }
        }
    }
    
    // Print the shortest distances
    printSolution(distance);
}
```

### 3. Floyd-Warshall Algorithm

The Floyd-Warshall algorithm finds the shortest paths between all pairs of vertices in a weighted graph.

```java
public void floydWarshall(int[][] graph) {
    int[][] dist = new int[V][V];
    
    // Initialize the distance matrix
    for (int i = 0; i < V; i++) {
        for (int j = 0; j < V; j++) {
            dist[i][j] = graph[i][j];
        }
    }
    
    // Update shortest paths considering each vertex as an intermediate
    for (int k = 0; k < V; k++) {
        for (int i = 0; i < V; i++) {
            for (int j = 0; j < V; j++) {
                if (dist[i][k] != Integer.MAX_VALUE && dist[k][j] != Integer.MAX_VALUE &&
                    dist[i][k] + dist[k][j] < dist[i][j]) {
                    dist[i][j] = dist[i][k] + dist[k][j];
                }
            }
        }
    }
    
    // Print the shortest distances between all pairs
    printAllPairsSolution(dist);
}

private void printAllPairsSolution(int[][] dist) {
    System.out.println("The following matrix shows the shortest distances between every pair of vertices");
    for (int i = 0; i < V; i++) {
        for (int j = 0; j < V; j++) {
            if (dist[i][j] == Integer.MAX_VALUE) {
                System.out.print("INF ");
            } else {
                System.out.print(dist[i][j] + " ");
            }
        }
        System.out.println();
    }
}
```

## Minimum Spanning Tree Algorithms

A Minimum Spanning Tree (MST) is a subset of the edges of a connected, edge-weighted undirected graph that connects all the vertices together, without any cycles and with the minimum possible total edge weight.

### 1. Prim's Algorithm

Prim's algorithm starts with a single vertex and grows the MST one edge at a time, always adding the minimum-weight edge that connects the tree to a new vertex.

```java
public void primMST() {
    int[] parent = new int[V]; // Store the MST
    int[] key = new int[V]; // Key values used to pick minimum weight edge
    boolean[] mstSet = new boolean[V]; // Represents the set of vertices included in MST
    
    // Initialize all keys as INFINITE
    for (int i = 0; i < V; i++) {
        key[i] = Integer.MAX_VALUE;
        mstSet[i] = false;
    }
    
    // Always include first vertex in MST
    key[0] = 0; // Make key 0 so that this vertex is picked as first vertex
    parent[0] = -1; // First node is always root of MST
    
    // The MST will have V vertices
    for (int count = 0; count < V - 1; count++) {
        // Pick the minimum key vertex from the set of vertices not yet included in MST
        int u = minKey(key, mstSet);
        
        // Add the picked vertex to the MST Set
        mstSet[u] = true;
        
        // Update key value and parent index of the adjacent vertices of the picked vertex
        for (Pair<Integer, Integer> edge : adj.get(u)) {
            int v = edge.getKey();
            int weight = edge.getValue();
            
            // Update the key only if v is not in mstSet, there is an edge from u to v, and the weight is smaller than key[v]
            if (!mstSet[v] && weight < key[v]) {
                parent[v] = u;
                key[v] = weight;
            }
        }
    }
    
    // Print the constructed MST
    printMST(parent, key);
}

private int minKey(int[] key, boolean[] mstSet) {
    int min = Integer.MAX_VALUE, minIndex = -1;
    
    for (int v = 0; v < V; v++) {
        if (!mstSet[v] && key[v] < min) {
            min = key[v];
            minIndex = v;
        }
    }
    
    return minIndex;
}

private void printMST(int[] parent, int[] key) {
    System.out.println("Edge \tWeight");
    for (int i = 1; i < V; i++) {
        System.out.println(parent[i] + " - " + i + "\t" + key[i]);
    }
}
```

### 2. Kruskal's Algorithm

Kruskal's algorithm builds the MST by adding edges one by one in increasing order of weight, as long as they don't form a cycle.

```java
public void kruskalMST(List<Edge> edges) {
    Collections.sort(edges, Comparator.comparingInt(e -> e.weight));
    
    DisjointSet ds = new DisjointSet(V);
    List<Edge> result = new ArrayList<>();
    
    for (Edge edge : edges) {
        int x = ds.find(edge.src);
        int y = ds.find(edge.dest);
        
        // If including this edge doesn't cause cycle, include it in result
        if (x != y) {
            result.add(edge);
            ds.union(x, y);
        }
    }
    
    // Print the edges of MST
    System.out.println("Edges in the constructed MST:");
    for (Edge edge : result) {
        System.out.println(edge.src + " -- " + edge.dest + " == " + edge.weight);
    }
}

static class Edge {
    int src, dest, weight;
    
    public Edge(int src, int dest, int weight) {
        this.src = src;
        this.dest = dest;
        this.weight = weight;
    }
}

static class DisjointSet {
    int[] parent, rank;
    
    public DisjointSet(int n) {
        parent = new int[n];
        rank = new int[n];
        
        for (int i = 0; i < n; i++) {
            parent[i] = i;
        }
    }
    
    public int find(int x) {
        if (parent[x] != x) {
            parent[x] = find(parent[x]); // Path compression
        }
        return parent[x];
    }
    
    public void union(int x, int y) {
        int rootX = find(x);
        int rootY = find(y);
        
        if (rootX == rootY) return;
        
        // Union by rank
        if (rank[rootX] < rank[rootY]) {
            parent[rootX] = rootY;
        } else if (rank[rootX] > rank[rootY]) {
            parent[rootY] = rootX;
        } else {
            parent[rootY] = rootX;
            rank[rootX]++;
        }
    }
}
```

## Topological Sorting

A topological sort of a directed acyclic graph (DAG) is a linear ordering of its vertices such that for every directed edge (u, v), vertex u comes before v in the ordering.

```java
public void topologicalSort() {
    Stack<Integer> stack = new Stack<>();
    boolean[] visited = new boolean[V];
    
    // Call the recursive helper function to store topological sort starting from all vertices
    for (int i = 0; i < V; i++) {
        if (!visited[i]) {
            topologicalSortUtil(i, visited, stack);
        }
    }
    
    // Print contents of stack
    System.out.println("Topological Sort:");
    while (!stack.isEmpty()) {
        System.out.print(stack.pop() + " ");
    }
}

private void topologicalSortUtil(int v, boolean[] visited, Stack<Integer> stack) {
    visited[v] = true;
    
    // Recur for all the vertices adjacent to this vertex
    for (int adjacentVertex : adj.get(v)) {
        if (!visited[adjacentVertex]) {
            topologicalSortUtil(adjacentVertex, visited, stack);
        }
    }
    
    // Push current vertex to stack which stores result
    stack.push(v);
}
```

### Kahn's Algorithm (BFS-based Topological Sort)

```java
public void kahnTopologicalSort() {
    // Calculate in-degree of all vertices
    int[] inDegree = new int[V];
    for (int i = 0; i < V; i++) {
        for (int vertex : adj.get(i)) {
            inDegree[vertex]++;
        }
    }
    
    // Create a queue and enqueue all vertices with in-degree 0
    Queue<Integer> queue = new LinkedList<>();
    for (int i = 0; i < V; i++) {
        if (inDegree[i] == 0) {
            queue.add(i);
        }
    }
    
    // Initialize count of visited vertices
    int count = 0;
    List<Integer> topOrder = new ArrayList<>();
    
    while (!queue.isEmpty()) {
        // Extract front of queue
        int u = queue.poll();
        topOrder.add(u);
        
        // Iterate through all its neighboring nodes and decrease their in-degree by 1
        for (int adjacentVertex : adj.get(u)) {
            // If in-degree becomes zero, add it to queue
            if (--inDegree[adjacentVertex] == 0) {
                queue.add(adjacentVertex);
            }
        }
        
        count++;
    }
    
    // Check if there was a cycle
    if (count != V) {
        System.out.println("There exists a cycle in the graph");
    } else {
        // Print topological order
        System.out.println("Topological Sort:");
        for (int i : topOrder) {
            System.out.print(i + " ");
        }
    }
}
```

## Strongly Connected Components

A strongly connected component (SCC) of a directed graph is a maximal strongly connected subgraph where every vertex is reachable from every other vertex within the subgraph.

### Kosaraju's Algorithm

Kosaraju's algorithm finds all strongly connected components in a directed graph.

```java
public void printSCCs() {
    Stack<Integer> stack = new Stack<>();
    boolean[] visited = new boolean[V];
    
    // Fill vertices in stack according to their finishing times
    for (int i = 0; i < V; i++) {
        if (!visited[i]) {
            fillOrder(i, visited, stack);
        }
    }
    
    // Create a reversed (or transpose) graph
    List<List<Integer>> reverseAdj = getTranspose();
    
    // Mark all the vertices as not visited (For second DFS)
    Arrays.fill(visited, false);
    
    // Process all vertices in order defined by Stack
    System.out.println("Strongly Connected Components:");
    while (!stack.isEmpty()) {
        int v = stack.pop();
        
        // Print SCCs of the reversed graph
        if (!visited[v]) {
            DFSUtil(reverseAdj, v, visited);
            System.out.println();
        }
    }
}

private void fillOrder(int v, boolean[] visited, Stack<Integer> stack) {
    visited[v] = true;
    
    for (int adjacentVertex : adj.get(v)) {
        if (!visited[adjacentVertex]) {
            fillOrder(adjacentVertex, visited, stack);
        }
    }
    
    stack.push(v);
}

private List<List<Integer>> getTranspose() {
    List<List<Integer>> transposeGraph = new ArrayList<>(V);
    for (int i = 0; i < V; i++) {
        transposeGraph.add(new ArrayList<>());
    }
    
    for (int v = 0; v < V; v++) {
        for (int adjacentVertex : adj.get(v)) {
            transposeGraph.get(adjacentVertex).add(v);
        }
    }
    
    return transposeGraph;
}

private void DFSUtil(List<List<Integer>> graph, int v, boolean[] visited) {
    visited[v] = true;
    System.out.print(v + " ");
    
    for (int adjacentVertex : graph.get(v)) {
        if (!visited[adjacentVertex]) {
            DFSUtil(graph, adjacentVertex, visited);
        }
    }
}
```

## Advanced Graph Algorithms

### 1. Tarjan's Algorithm for SCCs

Tarjan's algorithm is an alternative to Kosaraju's algorithm for finding strongly connected components, using a single DFS traversal.

```java
private int time = 0;

public void tarjanSCC() {
    int[] disc = new int[V];
    int[] low = new int[V];
    boolean[] stackMember = new boolean[V];
    Stack<Integer> stack = new Stack<>();
    
    // Initialize discovery times and low values
    for (int i = 0; i < V; i++) {
        disc[i] = -1;
        low[i] = -1;
    }
    
    // Call the recursive helper function to find SCCs
    System.out.println("Strongly Connected Components:");
    for (int i = 0; i < V; i++) {
        if (disc[i] == -1) {
            tarjanSCCUtil(i, disc, low, stackMember, stack);
        }
    }
}

private void tarjanSCCUtil(int u, int[] disc, int[] low, boolean[] stackMember, Stack<Integer> stack) {
    // Initialize discovery time and low value
    disc[u] = low[u] = ++time;
    stack.push(u);
    stackMember[u] = true;
    
    // Go through all vertices adjacent to this
    for (int v : adj.get(u)) {
        // If v is not visited yet, then recur for it
        if (disc[v] == -1) {
            tarjanSCCUtil(v, disc, low, stackMember, stack);
            
            // Check if the subtree rooted with v has a connection to
            // one of the ancestors of u
            low[u] = Math.min(low[u], low[v]);
        } 
        // Update low value of u for parent function calls
        else if (stackMember[v]) {
            low[u] = Math.min(low[u], disc[v]);
        }
    }
    
    // If u is head node of a SCC
    if (low[u] == disc[u]) {
        System.out.print("SCC: ");
        int v;
        do {
            v = stack.pop();
            stackMember[v] = false;
            System.out.print(v + " ");
        } while (v != u);
        System.out.println();
    }
}
```

### 2. Bridges in Graph

A bridge is an edge whose removal increases the number of connected components in a graph.

```java
public void findBridges() {
    boolean[] visited = new boolean[V];
    int[] disc = new int[V];
    int[] low = new int[V];
    int[] parent = new int[V];
    
    // Initialize parent, discovery time, and low value
    for (int i = 0; i < V; i++) {
        parent[i] = -1;
        visited[i] = false;
    }
    
    System.out.println("Bridges in the graph:");
    for (int i = 0; i < V; i++) {
        if (!visited[i]) {
            bridgeUtil(i, visited, disc, low, parent);
        }
    }
}

private void bridgeUtil(int u, boolean[] visited, int[] disc, int[] low, int[] parent) {
    // Mark the current node as visited
    visited[u] = true;
    
    // Initialize discovery time and low value
    disc[u] = low[u] = ++time;
    
    // Go through all vertices adjacent to this
    for (int v : adj.get(u)) {
        // If v is not visited yet, then recur for it
        if (!visited[v]) {
            parent[v] = u;
            bridgeUtil(v, visited, disc, low, parent);
            
            // Check if the subtree rooted with v has a connection to
            // one of the ancestors of u
            low[u] = Math.min(low[u], low[v]);
            
            // If the lowest vertex reachable from subtree under v is
            // below u in DFS tree, then u-v is a bridge
            if (low[v] > disc[u]) {
                System.out.println(u + " " + v);
            }
        }
        // Update low value of u for parent function calls
        else if (v != parent[u]) {
            low[u] = Math.min(low[u], disc[v]);
        }
    }
}
```

### 3. Articulation Points (Cut Vertices)

An articulation point is a vertex whose removal increases the number of connected components in a graph.

```java
public void findArticulationPoints() {
    boolean[] visited = new boolean[V];
    int[] disc = new int[V];
    int[] low = new int[V];
    boolean[] ap = new boolean[V]; // To store articulation points
    int[] parent = new int[V];
    
    // Initialize parent and visited arrays
    for (int i = 0; i < V; i++) {
        parent[i] = -1;
        visited[i] = false;
        ap[i] = false;
    }
    
    // Call the recursive helper function to find articulation points
    for (int i = 0; i < V; i++) {
        if (!visited[i]) {
            APUtil(i, visited, disc, low, parent, ap);
        }
    }
    
    // Print all articulation points
    System.out.println("Articulation points in the graph:");
    for (int i = 0; i < V; i++) {
        if (ap[i]) {
            System.out.print(i + " ");
        }
    }
}

private void APUtil(int u, boolean[] visited, int[] disc, int[] low, int[] parent, boolean[] ap) {
    // Mark the current node as visited
    visited[u] = true;
    
    // Initialize discovery time and low value
    disc[u] = low[u] = ++time;
    
    int children = 0;
    
    // Go through all vertices adjacent to this
    for (int v : adj.get(u)) {
        // If v is not visited yet, then make it a child of u in DFS tree and recur for it
        if (!visited[v]) {
            children++;
            parent[v] = u;
            APUtil(v, visited, disc, low, parent, ap);
            
            // Check if the subtree rooted with v has a connection to
            // one of the ancestors of u
            low[u] = Math.min(low[u], low[v]);
            
            // u is an articulation point in following cases:
            // (1) u is root of DFS tree and has two or more children
            if (parent[u] == -1 && children > 1) {
                ap[u] = true;
            }
            
            // (2) If u is not root and low value of one of its child is greater
            // than or equal to discovery value of u
            if (parent[u] != -1 && low[v] >= disc[u]) {
                ap[u] = true;
            }
        }
        // Update low value of u for parent function calls
        else if (v != parent[u]) {
            low[u] = Math.min(low[u], disc[v]);
        }
    }
}
```

## Common Interview Problems

### 1. Network Delay Time

**Problem**: Given a network of nodes, find the time it takes for all nodes to receive a signal sent from a given source node.

```java
public int networkDelayTime(int[][] times, int n, int k) {
    // Create adjacency list
    List<List<Pair<Integer, Integer>>> adj = new ArrayList<>();
    for (int i = 0; i <= n; i++) {
        adj.add(new ArrayList<>());
    }
    
    // Add edges to adjacency list
    for (int[] time : times) {
        int source = time[0];
        int target = time[1];
        int weight = time[2];
        adj.get(source).add(new Pair<>(target, weight));
    }
    
    // Distance array to store shortest path from source
    int[] distance = new int[n + 1];
    Arrays.fill(distance, Integer.MAX_VALUE);
    distance[k] = 0; // Source node
    
    // Min heap to get the node with shortest distance
    PriorityQueue<Pair<Integer, Integer>> minHeap = new PriorityQueue<>(
        Comparator.comparingInt(Pair::getValue)
    );
    minHeap.offer(new Pair<>(k, 0));
    
    while (!minHeap.isEmpty()) {
        Pair<Integer, Integer> current = minHeap.poll();
        int node = current.getKey();
        int dist = current.getValue();
        
        // If we've already found a shorter path to this node, skip
        if (dist > distance[node]) {
            continue;
        }
        
        // Process all neighbors
        for (Pair<Integer, Integer> neighbor : adj.get(node)) {
            int neighborNode = neighbor.getKey();
            int weight = neighbor.getValue();
            
            // If we found a shorter path to neighbor
            if (distance[node] + weight < distance[neighborNode]) {
                distance[neighborNode] = distance[node] + weight;
                minHeap.offer(new Pair<>(neighborNode, distance[neighborNode]));
            }
        }
    }
    
    // Find the maximum time (the bottleneck)
    int maxTime = 0;
    for (int i = 1; i <= n; i++) {
        if (distance[i] == Integer.MAX_VALUE) {
            return -1; // Not all nodes are reachable
        }
        maxTime = Math.max(maxTime, distance[i]);
    }
    
    return maxTime;
}
```

### 2. Course Schedule

**Problem**: Given the total number of courses and a list of prerequisite pairs, determine if it is possible to finish all courses (detect if there's a cycle in the directed graph).

```java
public boolean canFinish(int numCourses, int[][] prerequisites) {
    // Create adjacency list representation of the graph
    List<List<Integer>> adj = new ArrayList<>();
    for (int i = 0; i < numCourses; i++) {
        adj.add(new ArrayList<>());
    }
    
    // Add edges to the graph
    for (int[] prerequisite : prerequisites) {
        adj.get(prerequisite[1]).add(prerequisite[0]);
    }
    
    // To keep track of visited vertices during a DFS
    boolean[] visited = new boolean[numCourses];
    // To keep track of vertices in current recursion stack
    boolean[] recStack = new boolean[numCourses];
    
    // Check if there's a cycle
    for (int i = 0; i < numCourses; i++) {
        if (isCyclic(i, adj, visited, recStack)) {
            return false;
        }
    }
    
    return true;
}

private boolean isCyclic(int v, List<List<Integer>> adj, boolean[] visited, boolean[] recStack) {
    if (recStack[v]) {
        return true; // Cycle detected
    }
    
    if (visited[v]) {
        return false; // Already processed this vertex and no cycle was found
    }
    
    // Mark the current node as visited and part of recursion stack
    visited[v] = true;
    recStack[v] = true;
    
    // Recur for all the vertices adjacent to this vertex
    for (int adjacent : adj.get(v)) {
        if (isCyclic(adjacent, adj, visited, recStack)) {
            return true;
        }
    }
    
    // Remove the vertex from recursion stack
    recStack[v] = false;
    
    return false;
}
```

### 3. Number of Islands

**Problem**: Given a 2D grid map of '1's (land) and '0's (water), count the number of islands.

```java
public int numIslands(char[][] grid) {
    if (grid == null || grid.length == 0) {
        return 0;
    }
    
    int rows = grid.length;
    int cols = grid[0].length;
    int count = 0;
    
    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < cols; j++) {
            if (grid[i][j] == '1') {
                count++;
                dfs(grid, i, j);
            }
        }
    }
    
    return count;
}

private void dfs(char[][] grid, int row, int col) {
    int rows = grid.length;
    int cols = grid[0].length;
    
    // Check if out of bounds or if it's water or already visited
    if (row < 0 || col < 0 || row >= rows || col >= cols || grid[row][col] != '1') {
        return;
    }
    
    // Mark as visited by changing to '0'
    grid[row][col] = '0';
    
    // Check all four directions
    dfs(grid, row + 1, col);
    dfs(grid, row - 1, col);
    dfs(grid, row, col + 1);
    dfs(grid, row, col - 1);
}
```

### 4. Clone Graph

**Problem**: Given a reference of a node in a connected undirected graph, return a deep copy of the graph.

```java
class Node {
    public int val;
    public List<Node> neighbors;
    
    public Node(int val) {
        this.val = val;
        this.neighbors = new ArrayList<>();
    }
}

public Node cloneGraph(Node node) {
    if (node == null) {
        return null;
    }
    
    // HashMap to keep track of visited nodes and their clones
    Map<Node, Node> visited = new HashMap<>();
    
    // Use BFS to traverse the graph and clone nodes
    Queue<Node> queue = new LinkedList<>();
    queue.add(node);
    
    // Clone the root node
    visited.put(node, new Node(node.val));
    
    while (!queue.isEmpty()) {
        Node current = queue.poll();
        
        // Process all neighbors of current node
        for (Node neighbor : current.neighbors) {
            // If the neighbor hasn't been visited, clone it and add to queue
            if (!visited.containsKey(neighbor)) {
                visited.put(neighbor, new Node(neighbor.val));
                queue.add(neighbor);
            }
            
            // Add neighbor to the clone's neighbors list
            visited.get(current).neighbors.add(visited.get(neighbor));
        }
    }
    
    return visited.get(node);
}
```

### 5. Word Ladder

**Problem**: Given two words (beginWord and endWord), and a dictionary's word list, find the shortest transformation sequence from beginWord to endWord.

```java
public int ladderLength(String beginWord, String endWord, List<String> wordList) {
    Set<String> wordSet = new HashSet<>(wordList);
    
    // If endWord is not in the dictionary, there's no valid transformation
    if (!wordSet.contains(endWord)) {
        return 0;
    }
    
    // Queue for BFS
    Queue<String> queue = new LinkedList<>();
    queue.add(beginWord);
    
    // Set to keep track of visited words
    Set<String> visited = new HashSet<>();
    visited.add(beginWord);
    
    int level = 1;
    
    while (!queue.isEmpty()) {
        int size = queue.size();
        
        for (int i = 0; i < size; i++) {
            String currentWord = queue.poll();
            
            // Try changing each character of the current word
            char[] chars = currentWord.toCharArray();
            for (int j = 0; j < chars.length; j++) {
                char originalChar = chars[j];
                
                // Try replacing with all possible characters
                for (char c = 'a'; c <= 'z'; c++) {
                    chars[j] = c;
                    String newWord = new String(chars);
                    
                    // If we've reached the endWord, return the level
                    if (newWord.equals(endWord)) {
                        return level + 1;
                    }
                    
                    // If the new word is in the dictionary and hasn't been visited
                    if (wordSet.contains(newWord) && !visited.contains(newWord)) {
                        queue.add(newWord);
                        visited.add(newWord);
                    }
                }
                
                // Restore the original character
                chars[j] = originalChar;
            }
        }
        
        level++;
    }
    
    return 0; // No path found
}
```

## Practice Problems

1. **Pacific Atlantic Water Flow**

```java
public List<List<Integer>> pacificAtlantic(int[][] heights) {
    List<List<Integer>> result = new ArrayList<>();
    if (heights == null || heights.length == 0 || heights[0].length == 0) {
        return result;
    }
    
    int rows = heights.length;
    int cols = heights[0].length;
    
    boolean[][] pacificReachable = new boolean[rows][cols];
    boolean[][] atlanticReachable = new boolean[rows][cols];
    
    // DFS from Pacific and Atlantic edges
    for (int i = 0; i < rows; i++) {
        dfs(heights, pacificReachable, Integer.MIN_VALUE, i, 0);
        dfs(heights, atlanticReachable, Integer.MIN_VALUE, i, cols - 1);
    }
    
    for (int j = 0; j < cols; j++) {
        dfs(heights, pacificReachable, Integer.MIN_VALUE, 0, j);
        dfs(heights, atlanticReachable, Integer.MIN_VALUE, rows - 1, j);
    }
    
    // Find cells that can reach both oceans
    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < cols; j++) {
            if (pacificReachable[i][j] && atlanticReachable[i][j]) {
                result.add(Arrays.asList(i, j));
            }
        }
    }
    
    return result;
}

private void dfs(int[][] heights, boolean[][] reachable, int prevHeight, int row, int col) {
    int rows = heights.length;
    int cols = heights[0].length;
    
    // Check if out of bounds or already visited or can't flow (lower height)
    if (row < 0 || col < 0 || row >= rows || col >= cols || reachable[row][col] || heights[row][col] < prevHeight) {
        return;
    }
    
    reachable[row][col] = true;
    
    // Check all four directions
    dfs(heights, reachable, heights[row][col], row + 1, col);
    dfs(heights, reachable, heights[row][col], row - 1, col);
    dfs(heights, reachable, heights[row][col], row, col + 1);
    dfs(heights, reachable, heights[row][col], row, col - 1);
}
```

2. **Alien Dictionary**

```java
public String alienOrder(String[] words) {
    // Build the graph
    Map<Character, Set<Character>> graph = new HashMap<>();
    Map<Character, Integer> inDegree = new HashMap<>();
    
    // Initialize the graph and in-degree for all characters
    for (String word : words) {
        for (char c : word.toCharArray()) {
            graph.putIfAbsent(c, new HashSet<>());
            inDegree.putIfAbsent(c, 0);
        }
    }
    
    // Build the graph edges based on the order of words
    for (int i = 0; i < words.length - 1; i++) {
        String word1 = words[i];
        String word2 = words[i + 1];
        
        // Check that word2 is not a prefix of word1
        if (word1.length() > word2.length() && word1.startsWith(word2)) {
            return "";
        }
        
        // Find the first different character and add an edge
        for (int j = 0; j < Math.min(word1.length(), word2.length()); j++) {
            char c1 = word1.charAt(j);
            char c2 = word2.charAt(j);
            if (c1 != c2) {
                if (!graph.get(c1).contains(c2)) {
                    graph.get(c1).add(c2);
                    inDegree.put(c2, inDegree.get(c2) + 1);
                }
                break;
            }
        }
    }
    
    // Topological sort using BFS
    StringBuilder sb = new StringBuilder();
    Queue<Character> queue = new LinkedList<>();
    
    // Add all vertices with in-degree 0 to the queue
    for (Map.Entry<Character, Integer> entry : inDegree.entrySet()) {
        if (entry.getValue() == 0) {
            queue.add(entry.getKey());
        }
    }
    
    while (!queue.isEmpty()) {
        char c = queue.poll();
        sb.append(c);
        
        for (char neighbor : graph.get(c)) {
            inDegree.put(neighbor, inDegree.get(neighbor) - 1);
            if (inDegree.get(neighbor) == 0) {
                queue.add(neighbor);
            }
        }
    }
    
    // If the result doesn't include all characters, there's a cycle
    return sb.length() == inDegree.size() ? sb.toString() : "";
}
```

3. **Cheapest Flights Within K Stops**

```java
public int findCheapestPrice(int n, int[][] flights, int src, int dst, int k) {
    // Create the adjacency list
    List<List<int[]>> adj = new ArrayList<>();
    for (int i = 0; i < n; i++) {
        adj.add(new ArrayList<>());
    }
    
    for (int[] flight : flights) {
        adj.get(flight[0]).add(new int[]{flight[1], flight[2]});
    }
    
    // Initialize the distances array
    int[] distances = new int[n];
    Arrays.fill(distances, Integer.MAX_VALUE);
    distances[src] = 0;
    
    // Queue for BFS: {node, cost}
    Queue<int[]> queue = new LinkedList<>();
    queue.add(new int[]{src, 0});
    int stops = 0;
    
    while (!queue.isEmpty() && stops <= k) {
        int size = queue.size();
        
        for (int i = 0; i < size; i++) {
            int[] current = queue.poll();
            int node = current[0];
            int cost = current[1];
            
            for (int[] neighbor : adj.get(node)) {
                int nextNode = neighbor[0];
                int price = neighbor[1];
                
                // If the new cost is less than the current best
                if (cost + price < distances[nextNode]) {
                    distances[nextNode] = cost + price;
                    queue.add(new int[]{nextNode, cost + price});
                }
            }
        }
        
        stops++;
    }
    
    return distances[dst] == Integer.MAX_VALUE ? -1 : distances[dst];
}
```

4. **Word Search II**

```java
class TrieNode {
    TrieNode[] children = new TrieNode[26];
    String word = null;
}

public List<String> findWords(char[][] board, String[] words) {
    List<String> result = new ArrayList<>();
    if (board == null || board.length == 0 || words.length == 0) {
        return result;
    }
    
    // Build Trie
    TrieNode root = buildTrie(words);
    
    int rows = board.length;
    int cols = board[0].length;
    
    // Start DFS from each cell
    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < cols; j++) {
            dfs(board, i, j, root, result);
        }
    }
    
    return result;
}

private TrieNode buildTrie(String[] words) {
    TrieNode root = new TrieNode();
    for (String word : words) {
        TrieNode node = root;
        for (char c : word.toCharArray()) {
            int index = c - 'a';
            if (node.children[index] == null) {
                node.children[index] = new TrieNode();
            }
            node = node.children[index];
        }
        node.word = word;
    }
    return root;
}

private void dfs(char[][] board, int row, int col, TrieNode node, List<String> result) {
    int rows = board.length;
    int cols = board[0].length;
    
    // Check boundaries and if the current character is in the Trie
    if (row < 0 || col < 0 || row >= rows || col >= cols || board[row][col] == '#') {
        return;
    }
    
    char c = board[row][col];
    int index = c - 'a';
    if (node.children[index] == null) {
        return;
    }
    
    node = node.children[index];
    
    // If reached a word in the Trie
    if (node.word != null) {
        result.add(node.word);
        node.word = null; // To avoid adding duplicates
    }
    
    // Mark as visited
    board[row][col] = '#';
    
    // Visit neighbors
    dfs(board, row + 1, col, node, result);
    dfs(board, row - 1, col, node, result);
    dfs(board, row, col + 1, node, result);
    dfs(board, row, col - 1, node, result);
    
    // Restore the cell
    board[row][col] = c;
}
```

5. **Critical Connections in a Network (Bridges)**

```java
public List<List<Integer>> criticalConnections(int n, List<List<Integer>> connections) {
    List<List<Integer>> result = new ArrayList<>();
    List<List<Integer>> graph = new ArrayList<>();
    
    // Initialize the graph
    for (int i = 0; i < n; i++) {
        graph.add(new ArrayList<>());
    }
    
    // Build the graph
    for (List<Integer> connection : connections) {
        int u = connection.get(0);
        int v = connection.get(1);
        graph.get(u).add(v);
        graph.get(v).add(u);
    }
    
    int[] disc = new int[n];
    int[] low = new int[n];
    boolean[] visited = new boolean[n];
    
    // Initialize discovery and low time arrays
    Arrays.fill(disc, -1);
    
    // Start DFS from node 0
    dfs(0, -1, 0, disc, low, visited, graph, result);
    
    return result;
}

private void dfs(int u, int parent, int time, int[] disc, int[] low, boolean[] visited, 
                 List<List<Integer>> graph, List<List<Integer>> result) {
    // Mark the current node as visited
    visited[u] = true;
    disc[u] = low[u] = time;
    
    for (int v : graph.get(u)) {
        // If v is parent, skip
        if (v == parent) {
            continue;
        }
        
        // If v is not visited yet, recur for it
        if (!visited[v]) {
            dfs(v, u, time + 1, disc, low, visited, graph, result);
            
            // Check if the subtree rooted with v has a connection to
            // one of the ancestors of u
            low[u] = Math.min(low[u], low[v]);
            
            // If the lowest vertex reachable from subtree under v is
            // below u in DFS tree, then u-v is a bridge
            if (low[v] > disc[u]) {
                result.add(Arrays.asList(u, v));
            }
        } 
        // Update low value of u for parent function calls
        else {
            low[u] = Math.min(low[u], disc[v]);
        }
    }
}
```

## Conclusion

Graphs are versatile data structures that can model a wide range of real-world problems. Understanding graph algorithms is crucial for solving complex problems efficiently. This guide covered the fundamental concepts and implementations of various graph algorithms in Java, from basic traversals to advanced techniques for finding bridges and articulation points.

When preparing for interviews, it's important to practice implementing these algorithms and solving related problems. Focus on understanding the underlying principles rather than memorizing code. With a solid grasp of graph theory and algorithms, you'll be well-equipped to tackle a variety of challenging problems.