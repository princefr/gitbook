# Graph

## Concept

A **graph** (图) is a non-linear data structure composed of **vertices** (顶点) connected by **edges** (边). Vertices in a graph can be connected to any other vertex, and this connection is referred to as the **degree** (度) of the vertex.

A graph with directed edges is called a **directed graph** (有向图). For example, the relationship between followers and fans on Weibo forms a directed graph, where the degree of a vertex is divided into **out-degree** (出度) and **in-degree** (入度). In-degree represents the number of edges pointing to a vertex, i.e., the number of followers; out-degree represents the number of other vertices a vertex points to, i.e., the number of users being followed. A graph without directed edges is called an **undirected graph** (无向图), such as the friend relationship on WeChat.

A graph with weighted edges is called a **weighted graph** (带权图), such as the intimacy between QQ friends.

A graph where all vertices are connected is called a **connected graph** (连通图).

## Storage

### Adjacency Matrix

An **adjacency matrix** (邻接矩阵) relies on a two-dimensional array. For example:

![](../../.gitbook/assets/image%20%28110%29.png)

**Drawback**: Wastes storage space. For an undirected graph, half of the space is wasted. For a sparse graph, the waste is significant, such as in WeChat with hundreds of millions of users.

**Advantage**: Efficient when accessing vertex relationships; convenient for calculations, as many problems can be converted into matrix calculations.

### Adjacency List

An **adjacency list** (邻接表) relies on a one-dimensional array + lists. Each vertex corresponds to a list. For example:

![](../../.gitbook/assets/image%20%28227%29.png)

This is the principle of trading time for space. Although it saves a lot of space, it is relatively time-consuming to use.

```java
// Implementation of an undirected graph
public final class Graph {
    private final int v;
    private final List<Integer>[] adj;

    public Graph(int v) {
        this.v = v;
        adj = new List[v];
        for (int i = 0; i < v; i++) {
            adj[i] = new LinkedList<>();
        }
    }

    public void addEdge(int s, int t) {
        adj[s].add(t);
        adj[t].add(s);
    }
}
```

**Optimization**: Lists can be replaced with other data structures, such as red-black trees, skip lists, hash tables, ordered dynamic arrays (usable for binary search), etc.

{% hint style="info" %}
For a directed graph, the list stores the vertices pointed to by the vertex. Therefore, it is easy to find out which users a user follows. However, it is difficult to find the list of followers for a user. Therefore, an **inverse adjacency list** is needed, where vertices store the vertices pointing to them.
{% endhint %}

## BFS

Breadth-First Search (BFS) searches the nearest vertices first, then the next nearest ones.

```java
/**
* The paths found by BFS are the shortest paths.
*/
public void bfs(int s, int t) {
    int[] prev = new int[v];
    Arrays.fill(prev, -1);
    
    if (s == t) {
        print(prev, t);
    }
    
    boolean[] visited = new boolean[v];
    Arrays.fill(visited, false);
    visited[s] = true;
    
    Queue<Integer> queue = new LinkedList<>();
    queue.add(s);
    
    while (queue.size() > 0) {
        Integer p = queue.poll();
        for (int i : adj[p]) {
            if (!visited[i]) {
                prev[i] = p;
                if (i == t) {
                    print(prev, t);
                    return;
                }
                visited[i] = true;
                queue.add(i);
            }
        }
    }
}
```

Time complexity: O\(E\), Space complexity: O\(V\), where E is the number of edges, and V is the number of vertices.

### Example Problems

* [LeetCode 102: Binary Tree Level Order Traversal](https://github.com/StoneYunZhao/algorithm/blob/master/src/main/java/com/zhaoyun/leetcode/tree/LT102.java)

## DFS

Depth-First Search (DFS) uses backtracking and is highly suitable for recursive implementation.

```java
/**
* The paths found by DFS are not necessarily the shortest paths.
*/
public void dfs(int s, int t) {
    final AtomicReference<Boolean> found = new AtomicReference<>(false);
    
    boolean[] visited = new boolean[v];
    Arrays.fill(visited, false);
    
    int[] prev = new int[v];
    Arrays.fill(prev, -1);
    
    recurDfs(found, visited, prev, s, t);
    print(prev, t);
}

private void recurDfs(AtomicReference<Boolean> found, boolean[] visited, int[] prev, int s, int t) {
    if (visited[s] || found.get()) {
        return;
    }
    if (s == t) {
        found.set(true);
        return;
    }
    visited[s] = true;
    for (Integer i : adj[s]) {
        prev[i] = s;
        recurDfs(found, visited, prev, i, t);
    }
}
```

Time complexity: O\(E\), Space complexity: O\(V\), where E is the number of edges, and V is the number of vertices.

{% hint style="info" %}
DFS and BFS are the simplest two search methods, straightforward and without much optimization, also known as brute-force search algorithms. **Suitable for searching graphs with a small state space.**
{% endhint %}

### Example Problems

* [LeetCode 104: Maximum Depth of Binary Tree](https://github.com/StoneYunZhao/algorithm/blob/master/src/main/java/com/zhaoyun/leetcode/tree/LT104.java)
* [LeetCode 111: Minimum Depth of Binary Tree](https://github.com/StoneYunZhao/algorithm/blob/master/src/main/java/com/zhaoyun/leetcode/tree/LT111.java)