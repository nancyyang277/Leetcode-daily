# Reorder Routes to Make All Paths Lead to the City Zero
## Problem
There are n cities numbered from 0 to n - 1 and n - 1 roads such that there is only one way to travel between two different cities (this network form a tree). Last year, The ministry of transport decided to orient the roads in one direction because they are too narrow.

Roads are represented by connections where connections[i] = [ai, bi] represents a road from city ai to city bi.

This year, there will be a big event in the capital (city 0), and many people want to travel to this city.

Your task consists of reorienting some roads such that each city can visit the city 0. Return the minimum number of edges changed.

It's guaranteed that each city can reach city 0 after reorder.
<img width="473" alt="Screenshot 2024-07-28 at 1 02 47 PM" src="https://github.com/user-attachments/assets/5ac141dc-5540-4056-96b5-6c9be5c7e3de">

## Solution
```
class Solution {
    int count = 0;

    public void dfs(int node, int parent, Map<Integer, List<List<Integer>>> adj) {
        if (!adj.containsKey(node)) {
            return;
        }
        for (List<Integer> nei : adj.get(node)) {
            int neighbor = nei.get(0);
            int sign = nei.get(1);
            if (neighbor != parent) {
                count += sign;
                dfs(neighbor, node, adj);
            }
        }
    }

    public int minReorder(int n, int[][] connections) {
        Map<Integer, List<List<Integer>>> adj = new HashMap<>();
        for (int[] connection : connections) {
            adj.computeIfAbsent(connection[0], k -> new ArrayList<List<Integer>>()).add(
                    Arrays.asList(connection[1], 1));
            adj.computeIfAbsent(connection[1], k -> new ArrayList<List<Integer>>()).add(
                    Arrays.asList(connection[0], 0));
        }
        dfs(0, -1, adj);
        return count;
    }
}
```

## Summary
- Using DFS, build a tree with 0 as the root that if we want to move from any node to the root, all edges must be directed from a child to its parent
- count the number of edges in the tree rooted at node '0' that are directed from the parent node to a child node
- when construct the tree, we use a map that even though this problem provides directed graph, but we add an opposite edge from node b to node a for every given edge in connections from node a to node b. Let us refer to the edge we added as an "artificial" edge and the edge present in connections as an "original" edge.
- If we use an "artificial" edge to move from the parent node to the child node, we know that the original edge is directed from the child node to the parent node. We don't need to flip the "original" edge.
- we will associate an extra value with each edge that 1 for "original" edges and 0 for "artificial" edges.
- Now we start a DFS from node 0 and work our way down the tree (from parent to child). If we come across an "original" edge during the traversal, that is, an edge labeled with a 1, we increase the count by one. We don't modify count if we come across an "artificial" edge. We can combine these two operations and perform count += sign where sign is either 0 or 1 indicating an "artificial" or "original" edge.


# Surrounded regions
## Problem
You are given an m x n matrix board containing letters 'X' and 'O', capture regions that are surrounded:

Connect: A cell is connected to adjacent cells horizontally or vertically.
Region: To form a region connect every 'O' cell.
Surround: The region is surrounded with 'X' cells if you can connect the region with 'X' cells and none of the region cells are on the edge of the board.
A surrounded region is captured by replacing all 'O's with 'X's in the input matrix board.

<img width="483" alt="Screenshot 2024-07-28 at 1 50 50 PM" src="https://github.com/user-attachments/assets/aa657e8d-e474-4ffe-bab0-ee712ef21e46">

## Solution
```
class Solution {
    int[][] directions = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
    public void solve(char[][] board) {
        
        int m = board.length;
        int n = board[0].length;
        boolean[][] visited = new boolean[m][n];

        for (int i = 0; i < m; i++) {
            if (board[i][0] == 'O') {
                dfs(i, 0, visited, board);
            } 
            if (board[i][n-1] == 'O') {
                dfs(i, n-1, visited, board);
            }
        }
        
        for (int i = 0; i < n; i++) {
            if (board[0][i] == 'O') {
                dfs(0, i, visited, board);
            } 
            if (board[m-1][i] == 'O') {
                dfs(m-1, i, visited, board);
            }
        }
        
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (visited[i][j]) {
                    board[i][j] = 'O';
                } else {
                    board[i][j] = 'X';
                }
            }
        } 
    }

    public void dfs( int x, int y,  boolean[][] visited, char[][] board) {
        if (visited[x][y]) {
            return;
        }
        if (board[x][y] == 'O') {
            visited[x][y] = true;
        } else if (board[x][y] == 'X') {
            return;
        }
        for (int[] direction : directions) {
            int newX = x + direction[0];
            int newY = y + direction[1];
            if (newX < 0 || newY < 0 || newX >= board.length - 1 || newY >= board[0].length - 1) {
                continue;
            }
            dfs(newX, newY, visited, board);
        }
        
    }
}
```


## Summary
- first go over all the edge cells (on the border) that is currently 'O', then do a dfs search and if we encounter any 'O' in the search, we would mark visited[x][y] as true
   - visited[x][y] is true if it is originally 'O' and can be reached by a border 'O'
- loop through all the cells of the region and if the cell is visited (visited[x][y] is true), then mark it as 'O' else mark it as 'X'

