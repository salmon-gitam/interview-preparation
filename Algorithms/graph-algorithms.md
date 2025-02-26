Below are eight commonly asked graph algorithms along with a brief explanation, a sample interview‐style question, and a complete C solution with inline comments that explain each key step.

---

## 1. Depth-First Search (DFS)

**Explanation:**  
DFS explores as far along a branch as possible before backtracking. It is often implemented recursively.

**Interview Question:**  
*Write a C program to perform a depth-first search (DFS) on a graph represented as an adjacency matrix.*

**C Solution:**
```c
#include <stdio.h>
#define V 5  // Number of vertices in the graph

// Recursive utility function for DFS
void DFSUtil(int graph[V][V], int visited[], int v) {
    visited[v] = 1;              // Mark the current vertex as visited
    printf("%d ", v);            // Process the current vertex

    // Recur for all adjacent vertices that haven't been visited
    for (int i = 0; i < V; i++) {
        if (graph[v][i] && !visited[i])
            DFSUtil(graph, visited, i);
    }
}

int main() {
    // Example graph represented as an adjacency matrix
    int graph[V][V] = {
        {0, 1, 1, 0, 0},
        {1, 0, 0, 1, 0},
        {1, 0, 0, 1, 1},
        {0, 1, 1, 0, 1},
        {0, 0, 1, 1, 0}
    };

    int visited[V] = {0};  // Array to keep track of visited vertices

    printf("DFS traversal starting from vertex 0:\n");
    DFSUtil(graph, visited, 0);
    printf("\n");

    return 0;
}
```

---

## 2. Breadth-First Search (BFS)

**Explanation:**  
BFS explores all neighbors of a vertex before moving to the next level. It uses a queue to keep track of the next vertex to visit.

**Interview Question:**  
*Write a C program to perform a breadth-first search (BFS) on a graph represented as an adjacency matrix.*

**C Solution:**
```c
#include <stdio.h>
#define V 5  // Number of vertices

// Simple queue implementation using an array
int queue[V];
int front = 0, rear = -1;

// Enqueue an element into the queue
void enqueue(int x) {
    if (rear < V - 1) {
        queue[++rear] = x;
    }
}

// Dequeue an element from the queue
int dequeue() {
    if (front <= rear)
        return queue[front++];
    return -1; // Indicates an empty queue
}

// Check if the queue is empty
int isEmpty() {
    return front > rear;
}

int main() {
    // Graph represented as an adjacency matrix
    int graph[V][V] = {
        {0, 1, 1, 0, 0},
        {1, 0, 0, 1, 0},
        {1, 0, 0, 1, 1},
        {0, 1, 1, 0, 1},
        {0, 0, 1, 1, 0}
    };

    int visited[V] = {0};  // Array to track visited vertices
    int start = 0;         // Starting vertex for BFS

    // Mark the starting vertex as visited and enqueue it
    visited[start] = 1;
    enqueue(start);

    printf("BFS traversal starting from vertex %d:\n", start);
    while (!isEmpty()) {
        int v = dequeue();
        printf("%d ", v);

        // Enqueue all unvisited neighbors of v
        for (int i = 0; i < V; i++) {
            if (graph[v][i] && !visited[i]) {
                visited[i] = 1;
                enqueue(i);
            }
        }
    }
    printf("\n");
    return 0;
}
```

---

## 3. Dijkstra’s Algorithm

**Explanation:**  
Dijkstra’s algorithm finds the shortest path from a source to all vertices in a weighted graph with non-negative weights. It maintains a set of vertices whose minimum distance from the source is known.

**Interview Question:**  
*Write a C program to compute shortest paths from a source vertex using Dijkstra’s Algorithm on a weighted graph represented by an adjacency matrix.*

**C Solution:**
```c
#include <stdio.h>
#include <limits.h>
#define V 5  // Number of vertices

// Function to find the vertex with minimum distance not yet processed
int minDistance(int dist[], int sptSet[]) {
    int min = INT_MAX, min_index;
    for (int v = 0; v < V; v++) {
        if (!sptSet[v] && dist[v] <= min) {
            min = dist[v];
            min_index = v;
        }
    }
    return min_index;
}

// Dijkstra's algorithm implementation
void dijkstra(int graph[V][V], int src) {
    int dist[V];    // Array to hold shortest distances from src
    int sptSet[V];  // Boolean array to mark vertices included in the shortest path tree

    // Initialize all distances as INFINITE and sptSet[] as false
    for (int i = 0; i < V; i++) {
        dist[i] = INT_MAX;
        sptSet[i] = 0;
    }
    dist[src] = 0;  // Distance to the source is always 0

    // Find shortest path for all vertices
    for (int count = 0; count < V - 1; count++) {
        int u = minDistance(dist, sptSet);
        sptSet[u] = 1;  // Mark the vertex as processed

        // Update the distance value of adjacent vertices of u
        for (int v = 0; v < V; v++) {
            if (!sptSet[v] && graph[u][v] && dist[u] != INT_MAX 
                && dist[u] + graph[u][v] < dist[v]) {
                dist[v] = dist[u] + graph[u][v];
            }
        }
    }

    // Print the calculated shortest distances
    printf("Vertex \t Distance from Source\n");
    for (int i = 0; i < V; i++) {
        printf("%d \t %d\n", i, dist[i]);
    }
}

int main() {
    // Example weighted graph represented as an adjacency matrix
    int graph[V][V] = {
        {0, 10, 0, 30, 100},
        {10, 0, 50, 0, 0},
        {0, 50, 0, 20, 10},
        {30, 0, 20, 0, 60},
        {100, 0, 10, 60, 0}
    };
    int source = 0;
    dijkstra(graph, source);
    return 0;
}
```

---

## 4. Bellman-Ford Algorithm

**Explanation:**  
Bellman-Ford finds the shortest paths from a source in a weighted graph—even with negative edge weights—and can detect negative weight cycles.

**Interview Question:**  
*Write a C program to compute shortest paths from a source using the Bellman-Ford algorithm on a graph defined by its edge list.*

**C Solution:**
```c
#include <stdio.h>
#include <limits.h>
#define V 5   // Number of vertices
#define E 8   // Number of edges

// Structure to represent an edge in the graph
typedef struct {
    int src, dest, weight;
} Edge;

// Bellman-Ford algorithm implementation
void bellmanFord(Edge edges[], int src) {
    int dist[V];

    // Initialize distances from src to all vertices as infinite
    for (int i = 0; i < V; i++)
        dist[i] = INT_MAX;
    dist[src] = 0;  // Distance from source to itself is 0

    // Relax all edges V-1 times
    for (int i = 1; i <= V - 1; i++) {
        for (int j = 0; j < E; j++) {
            int u = edges[j].src;
            int v = edges[j].dest;
            int weight = edges[j].weight;
            if (dist[u] != INT_MAX && dist[u] + weight < dist[v])
                dist[v] = dist[u] + weight;
        }
    }

    // Check for negative-weight cycles
    for (int j = 0; j < E; j++) {
        int u = edges[j].src;
        int v = edges[j].dest;
        int weight = edges[j].weight;
        if (dist[u] != INT_MAX && dist[u] + weight < dist[v]) {
            printf("Graph contains negative weight cycle\n");
            return;
        }
    }

    // Print the computed shortest distances
    printf("Vertex   Distance from Source\n");
    for (int i = 0; i < V; i++)
        printf("%d \t\t %d\n", i, dist[i]);
}

int main() {
    // Define the edges of the graph
    Edge edges[E] = {
        {0, 1, -1}, {0, 2, 4},
        {1, 2, 3},  {1, 3, 2},
        {1, 4, 2},  {3, 2, 5},
        {3, 1, 1},  {4, 3, -3}
    };
    int source = 0;
    bellmanFord(edges, source);
    return 0;
}
```

---

## 5. Floyd-Warshall Algorithm

**Explanation:**  
Floyd-Warshall computes the shortest paths between every pair of vertices in a weighted graph (it can handle negative weights but no negative cycles).

**Interview Question:**  
*Write a C program to compute all-pairs shortest paths using the Floyd-Warshall algorithm on a graph represented by an adjacency matrix.*

**C Solution:**
```c
#include <stdio.h>
#define V 4
#define INF 99999  // A large value representing infinity

// Implementation of Floyd-Warshall algorithm
void floydWarshall(int graph[V][V]) {
    int dist[V][V];

    // Initialize the solution matrix with the input graph values
    for (int i = 0; i < V; i++)
        for (int j = 0; j < V; j++)
            dist[i][j] = graph[i][j];

    // Update the solution matrix by considering each vertex as an intermediate point
    for (int k = 0; k < V; k++) {
        for (int i = 0; i < V; i++) {
            for (int j = 0; j < V; j++) {
                if (dist[i][k] + dist[k][j] < dist[i][j])
                    dist[i][j] = dist[i][k] + dist[k][j];
            }
        }
    }

    // Print the shortest distance matrix
    printf("Shortest distances between every pair of vertices:\n");
    for (int i = 0; i < V; i++) {
        for (int j = 0; j < V; j++) {
            if (dist[i][j] == INF)
                printf("%7s", "INF");
            else
                printf("%7d", dist[i][j]);
        }
        printf("\n");
    }
}

int main() {
    // Graph represented as an adjacency matrix
    int graph[V][V] = {
        {0,   5,  INF, 10},
        {INF, 0,   3, INF},
        {INF, INF, 0,   1},
        {INF, INF, INF, 0}
    };
    floydWarshall(graph);
    return 0;
}
```

---

## 6. A* Search Algorithm

**Explanation:**  
A* search finds the shortest path between two nodes using a heuristic to guide its exploration. In grid-based searches, the Manhattan distance is a common heuristic.

**Interview Question:**  
*Write a C program that uses A* search on a 5x5 grid (0 = free cell, 1 = obstacle) to find a path from a start cell to a destination cell.*

**C Solution:**
> _Note:_ This simplified implementation uses an array-based open list (without a priority queue) and does not reconstruct the complete path.

```c
#include <stdio.h>
#include <stdlib.h>
#include <math.h>
#define ROW 5
#define COL 5
#define INF 9999

// Structure to hold details for each cell
typedef struct {
    int parentRow, parentCol;  // To store the cell's parent for path reconstruction
    float f, g, h;             // f = g + h; g = cost from start; h = heuristic cost to destination
} Cell;

// Check if cell (row, col) is valid
int isValid(int row, int col) {
    return (row >= 0) && (row < ROW) && (col >= 0) && (col < COL);
}

// Check if cell is unblocked (0 = free, 1 = obstacle)
int isUnBlocked(int grid[ROW][COL], int row, int col) {
    return grid[row][col] == 0;
}

// Check if the cell is the destination
int isDestination(int row, int col, int destRow, int destCol) {
    return row == destRow && col == destCol;
}

// Calculate the Manhattan distance heuristic
float calculateHValue(int row, int col, int destRow, int destCol) {
    return (float)(abs(row - destRow) + abs(col - destCol));
}

// A* Search algorithm implementation
void aStarSearch(int grid[ROW][COL], int startRow, int startCol, int destRow, int destCol) {
    if (!isValid(startRow, startCol) || !isValid(destRow, destCol)) {
        printf("Invalid start or destination.\n");
        return;
    }
    if (!isUnBlocked(grid, startRow, startCol) || !isUnBlocked(grid, destRow, destCol)) {
        printf("Start or destination is blocked.\n");
        return;
    }
    if (isDestination(startRow, startCol, destRow, destCol)) {
        printf("Already at destination.\n");
        return;
    }

    // Closed list: marks cells already evaluated
    int closedList[ROW][COL] = {0};

    // Initialize cell details for all cells
    Cell cellDetails[ROW][COL];
    for (int i = 0; i < ROW; i++)
        for (int j = 0; j < COL; j++) {
            cellDetails[i][j].f = INF;
            cellDetails[i][j].g = INF;
            cellDetails[i][j].h = INF;
            cellDetails[i][j].parentRow = -1;
            cellDetails[i][j].parentCol = -1;
        }

    // Initialize the starting cell
    cellDetails[startRow][startCol].f = 0.0;
    cellDetails[startRow][startCol].g = 0.0;
    cellDetails[startRow][startCol].h = 0.0;
    cellDetails[startRow][startCol].parentRow = startRow;
    cellDetails[startRow][startCol].parentCol = startCol;

    // Open list: using a simple array to store nodes (row, col, and f-value)
    typedef struct {
        int row, col;
        float f;
    } Node;
    Node *openList = (Node*)malloc(ROW * COL * sizeof(Node));
    int openCount = 0;
    openList[openCount].row = startRow;
    openList[openCount].col = startCol;
    openList[openCount].f = 0.0;
    openCount++;

    // Directions for movement: Up, Down, Left, Right
    int dir[4][2] = { {-1,0}, {1,0}, {0,-1}, {0,1} };

    // Main loop of A* search
    while (openCount > 0) {
        // Find the node with the smallest f value in the open list
        int minIndex = 0;
        for (int k = 1; k < openCount; k++) {
            if (openList[k].f < openList[minIndex].f)
                minIndex = k;
        }

        // Extract the cell with minimum f value
        int currentRow = openList[minIndex].row;
        int currentCol = openList[minIndex].col;

        // Remove this cell from the open list
        openList[minIndex] = openList[openCount - 1];
        openCount--;

        // Mark the current cell as processed
        closedList[currentRow][currentCol] = 1;

        // Process all valid neighbors (up, down, left, right)
        for (int d = 0; d < 4; d++) {
            int neighborRow = currentRow + dir[d][0];
            int neighborCol = currentCol + dir[d][1];

            if (isValid(neighborRow, neighborCol)) {
                // If destination is reached
                if (isDestination(neighborRow, neighborCol, destRow, destCol)) {
                    cellDetails[neighborRow][neighborCol].parentRow = currentRow;
                    cellDetails[neighborRow][neighborCol].parentCol = currentCol;
                    printf("Path found!\n");
                    free(openList);
                    return;
                }
                // If neighbor is unblocked and not already evaluated
                else if (!closedList[neighborRow][neighborCol] && isUnBlocked(grid, neighborRow, neighborCol)) {
                    float gNew = cellDetails[currentRow][currentCol].g + 1.0;
                    float hNew = calculateHValue(neighborRow, neighborCol, destRow, destCol);
                    float fNew = gNew + hNew;

                    // Update the neighbor cell if it has not been visited or a better path is found
                    if (cellDetails[neighborRow][neighborCol].f == INF || cellDetails[neighborRow][neighborCol].f > fNew) {
                        cellDetails[neighborRow][neighborCol].f = fNew;
                        cellDetails[neighborRow][neighborCol].g = gNew;
                        cellDetails[neighborRow][neighborCol].h = hNew;
                        cellDetails[neighborRow][neighborCol].parentRow = currentRow;
                        cellDetails[neighborRow][neighborCol].parentCol = currentCol;
                        // Add neighbor to the open list
                        openList[openCount].row = neighborRow;
                        openList[openCount].col = neighborCol;
                        openList[openCount].f = fNew;
                        openCount++;
                    }
                }
            }
        }
    }
    printf("Failed to find the Destination Cell\n");
    free(openList);
}

int main() {
    // 0 = free cell, 1 = obstacle
    int grid[ROW][COL] = {
        {0, 0, 0, 0, 0},
        {1, 1, 0, 1, 0},
        {0, 0, 0, 0, 0},
        {0, 1, 1, 1, 1},
        {0, 0, 0, 0, 0}
    };

    int startRow = 0, startCol = 0;
    int destRow = 4, destCol = 4;

    aStarSearch(grid, startRow, startCol, destRow, destCol);
    return 0;
}
```

---

## 7. Topological Sort

**Explanation:**  
Topological sort orders the vertices of a directed acyclic graph (DAG) such that for every directed edge _u → v_, vertex _u_ comes before _v_.

**Interview Question:**  
*Write a C program to perform a topological sort on a DAG using DFS.*

**C Solution:**
```c
#include <stdio.h>
#define V 6  // Number of vertices

// Recursive DFS function that pushes vertices onto a stack after exploring their neighbors
void topologicalSortUtil(int v, int visited[], int stack[], int *stackIndex, int graph[V][V]) {
    visited[v] = 1;  // Mark the current vertex as visited

    // Recur for all adjacent vertices
    for (int i = 0; i < V; i++) {
        if (graph[v][i] && !visited[i])
            topologicalSortUtil(i, visited, stack, stackIndex, graph);
    }
    // Push the current vertex to the stack
    stack[(*stackIndex)++] = v;
}

void topologicalSort(int graph[V][V]) {
    int visited[V] = {0};
    int stack[V];
    int stackIndex = 0;

    // Call the recursive helper function for all unvisited vertices
    for (int i = 0; i < V; i++) {
        if (!visited[i])
            topologicalSortUtil(i, visited, stack, &stackIndex, graph);
    }

    // Print the topological order by popping from the stack
    printf("Topological Sort: ");
    while (stackIndex > 0) {
        printf("%d ", stack[--stackIndex]);
    }
    printf("\n");
}

int main() {
    // Example DAG represented as an adjacency matrix.
    // For instance, edge 5->2, 5->0, 4->0, 4->1, 2->3, 3->1.
    int graph[V][V] = { {0, 0, 0, 0, 0, 0},
                        {0, 0, 0, 0, 0, 0},
                        {0, 0, 0, 1, 0, 0},
                        {0, 1, 0, 0, 0, 0},
                        {0, 1, 0, 0, 0, 0},
                        {1, 0, 1, 0, 0, 0} };

    topologicalSort(graph);
    return 0;
}
```

---

## 8. Minimum Spanning Tree (MST) Algorithms

### a. Kruskal’s Algorithm

**Explanation:**  
Kruskal’s algorithm builds an MST by sorting all the edges in non-decreasing order of weight and then adding edges one by one—using union–find to avoid cycles.

**Interview Question:**  
*Write a C program to construct the minimum spanning tree (MST) of a graph using Kruskal’s algorithm.*

**C Solution:**
```c
#include <stdio.h>
#include <stdlib.h>
#define V 4  // Number of vertices
#define E 5  // Number of edges

// Structure to represent an edge
typedef struct {
    int src, dest, weight;
} Edge;

// Structure for union-find subsets
typedef struct {
    int parent;
    int rank;
} Subset;

// Comparison function for sorting edges by weight
int compareEdges(const void* a, const void* b) {
    Edge* e1 = (Edge*)a;
    Edge* e2 = (Edge*)b;
    return e1->weight - e2->weight;
}

// Find function with path compression
int find(Subset subsets[], int i) {
    if (subsets[i].parent != i)
        subsets[i].parent = find(subsets, subsets[i].parent);
    return subsets[i].parent;
}

// Union function by rank
void unionSubsets(Subset subsets[], int x, int y) {
    int xroot = find(subsets, x);
    int yroot = find(subsets, y);
    if (subsets[xroot].rank < subsets[yroot].rank)
        subsets[xroot].parent = yroot;
    else if (subsets[xroot].rank > subsets[yroot].rank)
        subsets[yroot].parent = xroot;
    else {
        subsets[yroot].parent = xroot;
        subsets[xroot].rank++;
    }
}

// Kruskal's algorithm implementation
void kruskalMST(Edge edges[]) {
    // Sort edges in non-decreasing order
    qsort(edges, E, sizeof(Edge), compareEdges);

    // Allocate and initialize subsets for union-find
    Subset *subsets = (Subset*)malloc(V * sizeof(Subset));
    for (int v = 0; v < V; v++) {
        subsets[v].parent = v;
        subsets[v].rank = 0;
    }

    Edge result[V - 1];  // Array to store MST edges
    int e = 0;  // Index for result[]
    int i = 0;  // Index for sorted edges

    // Process sorted edges and add them if they don't form a cycle
    while (e < V - 1 && i < E) {
        Edge next_edge = edges[i++];
        int x = find(subsets, next_edge.src);
        int y = find(subsets, next_edge.dest);

        if (x != y) {
            result[e++] = next_edge;
            unionSubsets(subsets, x, y);
        }
    }

    // Print the resulting MST
    printf("Kruskal's MST:\n");
    for (i = 0; i < e; i++)
        printf("%d -- %d == %d\n", result[i].src, result[i].dest, result[i].weight);

    free(subsets);
}

int main() {
    // Graph edges: each edge is represented as (source, destination, weight)
    Edge edges[E] = {
        {0, 1, 10},
        {0, 2, 6},
        {0, 3, 5},
        {1, 3, 15},
        {2, 3, 4}
    };

    kruskalMST(edges);
    return 0;
}
```

### b. Prim’s Algorithm

**Explanation:**  
Prim’s algorithm builds an MST by starting from a chosen vertex and repeatedly adding the smallest edge that connects a vertex in the tree to a vertex outside the tree.

**Interview Question:**  
*Write a C program to construct the MST of a graph using Prim’s algorithm with an adjacency matrix.*

**C Solution:**
```c
#include <stdio.h>
#include <limits.h>
#define V 5  // Number of vertices

// Function to find the vertex with the minimum key value not yet included in MST
int minKey(int key[], int mstSet[]) {
    int min = INT_MAX, min_index;
    for (int v = 0; v < V; v++) {
        if (!mstSet[v] && key[v] < min) {
            min = key[v];
            min_index = v;
        }
    }
    return min_index;
}

// Prim's algorithm implementation to find MST
void primMST(int graph[V][V]) {
    int parent[V];   // Array to store constructed MST
    int key[V];      // Key values used to pick minimum weight edge
    int mstSet[V];   // Boolean array: true if vertex is included in MST

    // Initialize keys as infinite and mstSet[] as false
    for (int i = 0; i < V; i++) {
        key[i] = INT_MAX;
        mstSet[i] = 0;
    }
    key[0] = 0;      // Start with the first vertex
    parent[0] = -1;  // First vertex is the root of MST

    // MST will have V vertices
    for (int count = 0; count < V - 1; count++) {
        int u = minKey(key, mstSet);
        mstSet[u] = 1;  // Add the picked vertex to the MST set

        // Update key value and parent index for adjacent vertices
        for (int v = 0; v < V; v++) {
            if (graph[u][v] && !mstSet[v] && graph[u][v] < key[v]) {
                parent[v] = u;
                key[v] = graph[u][v];
            }
        }
    }

    // Print the constructed MST
    printf("Prim's MST:\n");
    for (int i = 1; i < V; i++)
        printf("%d - %d  (weight: %d)\n", parent[i], i, graph[i][parent[i]]);
}

int main() {
    // Graph represented as an adjacency matrix
    int graph[V][V] = {
        {0, 2, 0, 6, 0},
        {2, 0, 3, 8, 5},
        {0, 3, 0, 0, 7},
        {6, 8, 0, 0, 9},
        {0, 5, 7, 9, 0},
    };

    primMST(graph);
    return 0;
}
```

---
