### 2021.6.30

My solution

    class Solution {
        public int numIslands(char[][] grid) {
            int m = grid.length;
            int n = grid[0].length;
            
            int count = 0;
            for (int i = 0; i < m; i++) {
                for (int j = 0; j < n; j++) {
                    if (grid[i][j] == '1') {
                        dfs(grid, i, j);
                        count++;
                    }
                }
            }
            
            return count;
        }
        
        private void dfs(char[][] grid, int i, int j) {
            if (i < 0 || i >= grid.length || j < 0 || j >= grid[0].length) {
                return;
            }
            
            if (grid[i][j] == '0' || grid[i][j] == '#') {
                return;
            }
            
            grid[i][j] = '#';
            int[][] directions = new int[][]{{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
            
            for (int[] direction : directions) {
                dfs(grid, i + direction[0], j + direction[1]);
            }
        }
    }

Solution 2:

    class Solution {
        public int numIslands(char[][] grid) {
            int m = grid.length;
            int n = grid[0].length;
            
            int count = 0;
            for (int i = 0; i < m; i++) {
                for (int j = 0; j < n; j++) {
                    if (grid[i][j] == '1') {
                        bfs(grid, i, j);
                        count++;
                    }
                }
            }
            
            return count;
        }
        
        private void bfs(char[][] grid, int i, int j) {
            int m = grid.length;
            int n = grid[0].length;
            int[][] directions = new int[][]{{-1, 0}, {1, 0}, {0, 1}, {0, -1}};
            
            Queue<Integer> queue = new LinkedList<>();
            queue.add(i * n + j);
            
            while (!queue.isEmpty()) {
                int index = queue.poll();
                
                int row = index / n;
                int col = index % n;
                grid[row][col] = '0';
                
                for (int[] direction : directions) {
                    int newRow = row + direction[0];
                    int newCol = col + direction[1];
                    
                    if (newRow >= 0 && newRow < m && newCol >= 0 && newCol < n && grid[newRow][newCol] == '1') {
                        grid[newRow][newCol] = '0';
                        queue.add(newRow * n + newCol);
                    }
                }
            }
        }
    }

Solution 3:

    class Solution {
        class UnionFind {
            int[] parent;
            int count;
            int m;
            int n;
            
            UnionFind(char[][] grid) {
                m = grid.length;
                n = grid[0].length;
                
                parent = new int[m * n];
                count = 0;
                for (int i = 0; i < m; i++) {
                    for (int j = 0; j < n; j++) {
                        if (grid[i][j] == '1') {
                            parent[i * n + j] = -1;
                            count++;
                        }
                    }
                }
            }
            
            int find(int index) {
                if (parent[index] < 0) {
                    return index;
                }
                
                parent[index] = find(parent[index]);
                
                return parent[index];
            }
            
            void connect(int i1, int j1, int i2, int j2) {
                int index1 = i1 * n + j1;
                int index2 = i2 * n + j2;
                
                int root1 = find(index1);
                int root2 = find(index2);
                
                if (root1 != root2) {
                    if (Math.abs(parent[root1]) >= Math.abs(parent[root2])) {
                        parent[root1] += parent[root2];
                        parent[root2] = root1;
                    } else {
                        parent[root2] += parent[root1];
                        parent[root1] = root2;
                    }
                    
                    count--;
                }
            }
            
            int getCount() {
                return count;
            }
        }
        
        public int numIslands(char[][] grid) {
            UnionFind uf = new UnionFind(grid);
            
            int m = grid.length;
            int n = grid[0].length;
            for (int i = 0; i < m; i++) {
                for (int j = 0; j < n; j++) {
                    if (grid[i][j] == '0') {
                        continue;
                    }
                    
                    if (i - 1 >= 0 && grid[i - 1][j] == '1') {
                        uf.connect(i, j, i - 1, j);
                    }
                    
                    if (j - 1 >= 0 && grid[i][j - 1] == '1') {
                        uf.connect(i, j, i, j - 1);
                    }
                }
            }
            
            return uf.getCount();
        }
    }