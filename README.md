# As far from land as possible
## https://leetcode.com/problems/as-far-from-land-as-possible

Given an n x n grid containing only values 0 and 1, where 0 represents water and 1 represents land, find a water cell such that its distance to the nearest land cell is maximized, and return the distance. If no land or water exists in the grid, return -1.

The distance used in this problem is the Manhattan distance: the distance between two cells (x0, y0) and (x1, y1) is |x0 - x1| + |y0 - y1|.

!["As far from land as possible"](example.JPG?raw=true)

### Constraints:
```
1. n == grid.length
2. n == grid[i].length
3. 1 <= n <= 100
4. grid[i][j] is 0 or 1
```

# Implementation 1 : Naive : Time Limit exceeded
Calculate the min distance to land for each 0 cell, and return the furthest (max distance) from land as answer.

```java
class Solution {
    public int maxDistance(int[][] grid) {
        if(grid == null || grid.length == 0)
            return -1;
        int rows = grid.length;
        int cols = grid[0].length;
        
        int furthest = -1;
        List<int[]> lands = new ArrayList<>();
        for(int i = 0; i < rows; i++) {
            for(int j = 0; j < cols; j++) {
                if(grid[i][j] == 1)
                    lands.add(new int[]{i,j});
            }
        }
        if(lands.size() == 0)
            return -1;
        for(int i = 0; i < rows; i++) {
            for(int j = 0; j < cols; j++) {
                if(grid[i][j] == 0) {
                    int minDistance = Integer.MAX_VALUE;
                    for(int[] land : lands) {
                        int distance = Math.abs(i-land[0]) + Math.abs(j-land[1]);
                        minDistance = Math.min(minDistance, distance);
                    }
                    furthest = Math.max(furthest, minDistance);
                }
            }
        }
       return furthest;
    }
}

```

## Implementation 2 : BFS starting from Lands
```java
class Solution {
    public int maxDistance(int[][] grid) {
        if(grid == null || grid.length == 0)
            return -1;
        int rows = grid.length;
        int cols = grid[0].length;
        
        Queue<int[]> q = new ArrayDeque<>();
        Set<String> visited = new HashSet<>();
        for(int i = 0; i < rows; i++) {
            for(int j = 0; j < cols; j++) {
                if(grid[i][j] == 1) {
                    q.offer(new int[]{i,j});
                    visited.add(i+","+j);
                }
            }
        }
        int distance = 0;
        int[][] directions = {{0, 1}, {0, -1}, {-1, 0}, {1, 0}};
        while(!q.isEmpty()) {
            int size = q.size();
            boolean nextLevel = false;
            for(int i = 0; i < size; i++) {
                int[] pos = q.remove();
                for(int[] direction : directions) {
                    int x = pos[0] + direction[0];
                    int y = pos[1] + direction[1];
                    if(x >= 0 && x < rows && y >= 0 && y < cols && 
                       grid[x][y] == 0 && !visited.contains(x+","+y)) {
                        q.offer(new int[]{x,y});
                        visited.add(x+","+y);
                        nextLevel = true;
                    }
                }
            }
            if(nextLevel)
              distance++;
              
        }
        return distance == 0 ? -1 : distance;
    }
}
```
