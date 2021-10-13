# As far from land as possible

# Implementation 1 : Naive
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
