<<<<<<< HEAD
# React + Vite

This template provides a minimal setup to get React working in Vite with HMR and some ESLint rules.

Currently, two official plugins are available:

- [@vitejs/plugin-react](https://github.com/vitejs/vite-plugin-react/blob/main/packages/plugin-react) uses [Oxc](https://oxc.rs)
- [@vitejs/plugin-react-swc](https://github.com/vitejs/vite-plugin-react/blob/main/packages/plugin-react-swc) uses [SWC](https://swc.rs/)

## React Compiler

The React Compiler is not enabled on this template because of its impact on dev & build performances. To add it, see [this documentation](https://react.dev/learn/react-compiler/installation).

## Expanding the ESLint configuration

If you are developing a production application, we recommend using TypeScript with type-aware lint rules enabled. Check out the [TS template](https://github.com/vitejs/vite/tree/main/packages/create-vite/template-react-ts) for information on how to integrate TypeScript and [`typescript-eslint`](https://typescript-eslint.io) in your project.
=======
# demo
.md => marked down


<p> git add .  </p>

<p> git commit -m </p>

<p> git push </p>

** git clone https://github.com/vishalkumarjha192/demo.git **









# Unit 4 Assignment: Trees & Graphs (Modeling, Traversals, Applications)

**Student Name:** _______________  
**Roll No.:** _______________  
**Section:** _______________  
**Date:** _______________

---

## Task 1: Conceptual Clarity & Terminology

### 1.1 Definitions

| Term | Definition |
|------|-------------|
| **Root** | The topmost node of a tree with no parent |
| **Leaf** | A node with no children (degree 0) |
| **Internal node** | A node that has at least one child |
| **Edge** | A connection/link between a parent node and its child |
| **Degree** | The number of children a node has |
| **Level** | Root at level 0, children at level 1, etc. |
| **Height** | Maximum number of edges from a node to a leaf (tree height = root's height) |
| **Depth** | Number of edges from root to a given node |

### 1.2 Example Tree Diagram

```
                    A (root, degree=2)
                   / \
                  B   C
                 / \   \
                D   E   F
               /       / \
              G       H   I
```

**Analysis:**
- **Number of edges:** 8 (since 9 nodes - 1 = 8 edges)
- **Height of the tree:** 3 (A → B → D → G or A → C → F → H)
- **Degree of root (A):** 2
- **Leaf nodes:** G, E, H, I

### 1.3 Why Trees for Modeling Hierarchies?

Trees naturally represent **parent-child relationships** where each node (except root) has exactly one parent. This one-to-many relationship maps perfectly to hierarchical structures.

**Real examples:**
1. **File System:** Root directory → subdirectories → files
2. **Organization Chart:** CEO → Department Heads → Managers → Employees

---

## Task 2: Binary Tree Traversals

### Given Binary Tree:

```
                    1
                   / \
                  2   3
                 / \   \
                4   5   6
```

### 2.1 Traversal Outputs

| Traversal | Output |
|-----------|--------|
| **Preorder** (Root→Left→Right) | 1, 2, 4, 5, 3, 6 |
| **Inorder** (Left→Root→Right) | 4, 2, 5, 1, 3, 6 |
| **Postorder** (Left→Right→Root) | 4, 5, 2, 6, 3, 1 |
| **Level-order** (BFS) | 1, 2, 3, 4, 5, 6 |

### 2.2 Level-order Queue State (First 5 steps)

| Step | Dequeue (visit) | Queue after enqueuing children |
|------|----------------|-------------------------------|
| Initial | - | [1] |
| 1 | 1 | [2, 3] |
| 2 | 2 | [3, 4, 5] |
| 3 | 3 | [4, 5, 6] |
| 4 | 4 | [5, 6] |
| 5 | 5 | [6] |

### 2.3 Complexity Analysis

| Traversal | Time Complexity | Auxiliary Space |
|-----------|----------------|-----------------|
| Preorder (recursive) | O(n) | O(h) where h = height |
| Inorder (recursive) | O(n) | O(h) |
| Postorder (recursive) | O(n) | O(h) |
| Level-order (queue) | O(n) | O(w) where w = max width |

**Reasoning:** Each node is visited exactly once → O(n).  
For recursive traversals, call stack depth = tree height.  
For level-order, queue size = maximum number of nodes at any level.

---

## Task 3: Reconstruct Binary Tree from Traversals

### 3.1 Manual Reconstruction Steps

**Given:**
- Preorder = [A, B, D, E, C, F]
- Inorder = [D, B, E, A, C, F]

**Step 1:** Root = first element of preorder = **A**
- In inorder: elements left of A = [D, B, E] → left subtree
- In inorder: elements right of A = [C, F] → right subtree

**Step 2:** Next in preorder after A = **B** (root of left subtree)
- In inorder left part [D, B, E]: D left of B, E right of B

**Step 3:** Next in preorder after B = **D** (leaf node)
- D has no children in inorder left part

**Step 4:** Next in preorder after D = **E** (leaf node)

**Step 5:** Next in preorder after E = **C** (root of right subtree)
- In inorder right part [C, F]: F right of C

**Final Tree Diagram:**

```
                    A
                   / \
                  B   C
                 / \   \
                D   E   F
```

### 3.2 Implementation Approach

```python
def build_tree(preorder, inorder):
    if not preorder:
        return None
    root = TreeNode(preorder[0])
    idx = inorder.index(preorder[0])
    root.left = build_tree(preorder[1:idx+1], inorder[:idx])
    root.right = build_tree(preorder[idx+1:], inorder[idx+1:])
    return root
```

**Verification:** After building, inorder traversal returns [D,B,E,A,C,F] ✓

### 3.3 Usefulness of Tree Reconstruction

- **Expression Trees:** Parse mathematical expressions from prefix/infix notation
- **Compilers:** Build Abstract Syntax Trees (AST) from token streams
- **Data Serialization:** Save/restore tree structures to/from file/network
- **Version Control:** Reconstruct directory trees from change logs

---

## Task 4: Binary Search Tree (BST)

### 4.1 BST Operations Implemented

| Operation | Approach | Time Complexity |
|-----------|----------|-----------------|
| insert(x) | Compare with root, recurse left/right | O(h) |
| search(x) | Compare and traverse | O(h) |
| delete(x) | 3 cases (0,1,2 children) | O(h) |
| find_min() | Go left until null | O(h) |
| find_max() | Go right until null | O(h) |
| inorder() | Left→Root→Right recursion | O(n) |

### 4.2 BST Numerical Reasoning

**Insert order:** [50, 30, 70, 20, 40, 60, 80]

**After all inserts - Inorder:** 20, 30, 40, 50, 60, 70, 80 ✓ (sorted)

**Delete operations:**
| Operation | Inorder Output |
|-----------|----------------|
| Delete 20 (leaf) | 30, 40, 50, 60, 70, 80 |
| Delete 30 (one child) | 40, 50, 60, 70, 80 |
| Delete 50 (two children) | 40, 60, 70, 80 |

**Search results:**
- search(60) → **True**
- search(100) → **False**

### 4.3 Performance Reflection

**Problem:** When keys are inserted in **sorted order**, BST becomes a **skewed tree** (linked list).

**Example:** Insert [10, 20, 30, 40, 50]
- Height becomes 5 instead of ~3 for balanced tree
- Search becomes O(n) instead of O(log n)

**Impact:**
- Balanced BST height ≈ log₂(n) → O(log n) operations
- Skewed BST height = n → O(n) operations

**Solution:** Use self-balancing trees (AVL, Red-Black) or randomize insertion order.

---

## Task 5: Graph Modeling

### 5.1 Definitions

| Term | Definition |
|------|-------------|
| **Vertex** | A node in the graph |
| **Edge** | Connection between two vertices |
| **Directed graph** | Edges have direction (u→v ≠ v→u) |
| **Undirected graph** | Edges are bidirectional |
| **Weighted graph** | Edges have numeric costs |
| **Path** | Sequence of vertices connected by edges |
| **Cycle** | Path that starts and ends at same vertex |
| **Connected component** | Maximal set of mutually reachable vertices |

### 5.2 Graph Representation

**Vertices:** {0, 1, 2, 3, 4}  
**Edges:** (0,1), (0,2), (1,2), (2,3), (3,4)

**Adjacency List:**
```
0 → [1, 2]
1 → [0, 2]
2 → [0, 1, 3]
3 → [2, 4]
4 → [3]
```

**Adjacency Matrix (5×5):**

```
    0  1  2  3  4
0   0  1  1  0  0
1   1  0  1  0  0
2   1  1  0  1  0
3   0  0  1  0  1
4   0  0  0  1  0
```

### 5.3 Adjacency List vs Matrix

| Criteria | Adjacency List | Adjacency Matrix |
|----------|---------------|------------------|
| **Space** | O(V + E) | O(V²) |
| **Edge lookup** | O(degree) | O(1) |
| **Add edge** | O(1) | O(1) |
| **Iterate neighbors** | O(degree) | O(V) |
| **Best for** | Sparse graphs (E << V²) | Dense graphs (E ≈ V²) |

**Prefer adjacency list when:**
- Graph is sparse (most real-world graphs)
- Memory is limited
- Need fast neighbor iteration (BFS/DFS)

---

## Task 6: BFS and DFS Implementation

### 6.1 BFS from Node 0

**Graph:** 0—1, 0—2, 1—2, 2—3, 3—4

**BFS Order:** [0, 1, 2, 3, 4]

**Queue States:**

| Step | Dequeue (visit) | Queue (after enqueuing unvisited neighbors) |
|------|----------------|---------------------------------------------|
| Start | - | [0] |
| 1 | 0 | [1, 2] |
| 2 | 1 | [2] (neighbors 0,2; 0 visited, 2 already in queue) |
| 3 | 2 | [3] |
| 4 | 3 | [4] |
| 5 | 4 | [] |

### 6.2 DFS from Node 0 (Recursive)

**DFS Order:** [0, 1, 2, 3, 4] (depends on adjacency order)

**Recursion Stack concept:**
```
dfs(0) calls dfs(1)
  dfs(1) calls dfs(2)
    dfs(2) calls dfs(3)
      dfs(3) calls dfs(4)
        dfs(4) returns
      returns
    returns
  returns
returns
```

### 6.3 Connected Components

**Disconnected graph:**  
Vertices = {0,1,2,3,4,5}  
Edges = (0,1), (1,2), (3,4)

**Components:**
- Component 1: [0, 1, 2]
- Component 2: [3, 4]
- Component 3: [5] (isolated vertex)

**How BFS/DFS finds components:**
1. Keep a `visited` array
2. For each unvisited vertex, run BFS/DFS
3. All vertices discovered in that traversal form one component
4. Repeat until all vertices visited

### 6.4 Complexity O(V + E)

**Reasoning:**
- Each vertex is enqueued/dequeued once → O(V)
- Each edge is examined twice (for undirected) → O(E)
- Total operations = O(V + E)

This is optimal because you must at least look at every vertex and edge.

---

## Task 7: Shortest Path in Unweighted Graph

### 7.1 Implementation Approach

Using BFS with parent tracking:

```python
def shortest_path(adj, src, dst):
    visited = [False] * len(adj)
    parent = [-1] * len(adj)
    queue = [src]
    visited[src] = True
    
    while queue:
        u = queue.pop(0)
        if u == dst:
            # reconstruct path
            path = []
            while u != -1:
                path.append(u)
                u = parent[u]
            return path[::-1], len(path) - 1
        for v in adj[u]:
            if not visited[v]:
                visited[v] = True
                parent[v] = u
                queue.append(v)
    return None, -1  # no path
```

### 7.2 Shortest Path 0 → 4

**Path:** [0, 2, 3, 4]  
**Distance:** 3 edges

**Why BFS guarantees shortest path in unweighted graphs:**
- BFS explores vertices in **layers** (distance 0, then 1, then 2, etc.)
- The first time we reach a vertex, we've found the **minimum number of edges**
- This property holds because all edges have equal weight (1)

---

## Required Test Cases - Expected Outputs

### T1: Binary Tree Traversals
```
Preorder: 1 2 4 5 3 6
Inorder: 4 2 5 1 3 6
Postorder: 4 5 2 6 3 1
Level-order: 1 2 3 4 5 6
```

### T2: Build Tree from Traversals
```
Constructed tree inorder: D B E A C F
Constructed tree preorder: A B D E C F
✅ Matches input traversals
```

### T3: BST Operations
```
Initial inorder: 20 30 40 50 60 70 80
After delete 20: 30 40 50 60 70 80
After delete 30: 40 50 60 70 80
After delete 50: 40 60 70 80
Search 60: True
Search 100: False
```

### T4: Graph BFS/DFS
```
BFS from 0: [0, 1, 2, 3, 4]
DFS from 0: [0, 1, 2, 3, 4]
```

### T5: Connected Components
```
Components: [[0, 1, 2], [3, 4], [5]]
```

### T6: Shortest Path
```
Shortest path 0→4: [0, 2, 3, 4]
Distance: 3
```

---

## Reflection & Conclusion

### Key Learnings

1. **Trees vs Graphs:** Trees are special graphs (acyclic, connected) ideal for hierarchies
2. **Traversal importance:** Different orders serve different purposes (expression evaluation, sorting, serialization)
3. **BST trade-offs:** Simple but degenerate to O(n) without balancing
4. **Graph representation choice:** List for sparse, matrix for dense — affects all algorithms
5. **BFS vs DFS:** BFS for shortest path, DFS for cycle detection / topological sort

### Complexity Summary Table

| Data Structure / Algorithm | Time | Space |
|---------------------------|------|-------|
| Tree traversals (all) | O(n) | O(h) / O(w) |
| BST search/insert/delete | O(h) | O(1) |
| BFS/DFS on graph | O(V+E) | O(V) |
| Shortest path (unweighted) | O(V+E) | O(V) |

---


>>>>>>> 982f7106b4c502c491dd99edebfa5dfdb9b58be5
