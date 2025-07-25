### [695. 岛屿的最大面积](https://leetcode.cn/problems/max-area-of-island/)
- DFS
```cpp
// False
class Solution {  
public:  
    int maxAreaOfIsland(vector<vector<int>>& grid) {  
        int rows = grid.size();  
        int cols = grid[0].size();  
        int res = 0;  
        unordered_map<int, int> temp;  // 值都是 0/1 无法正确判断是否访问过
        for (int i = 0; i < rows; i++) {  
            for (int j = 0; j < cols; j++) {  
                if (grid[i][j] != 0 && !temp.count(grid[i][j])) {  // 同上
                    int s = 0;  
                    temp[grid[i][j]]++;  //
                    DFS(grid, s, i, j, rows, cols);  
                    res = max(s, res);  
                }  
            }  
        }  
        return res;  
    }  
  
    void DFS(vector<vector<int>> &grid, int &s, int row, int col, int rows, int cols) {    // 未判断是否合法便使用，导致越界， 应放最后检查
        if (grid[row][col] == 0 || row < 0 || row > rows || col < 0 || col > cols)  
            return;  
        s++;  
        DFS(grid, s, row + 1, col, rows, cols);  
        DFS(grid, s, row - 1, col, rows, cols);  
        DFS(grid, s, row, col + 1, rows, cols);  
        DFS(grid, s, row, col - 1, rows, cols);  
    }  
};

// True
class Solution {  
public:  
    int maxAreaOfIsland(vector<vector<int>>& grid) {  
        int rows = grid.size();  
        int cols = grid[0].size();  
        int res = 0;  
        for (int i = 0; i < rows; i++) {  
            for (int j = 0; j < cols; j++) {  
                if (grid[i][j] != 0) {  
                    int s = 0;  
                    DFS(grid, s, i, j, rows, cols);  
                    res = max(s, res);  
                }  
            }  
        }  
        return res;  
    }  
  
    void DFS(vector<vector<int>> &grid, int &s, int row, int col, int rows, int cols) {  // 修改边界检查
        if (row < 0 || row >= rows || col < 0 || col >= cols || grid[row][col] == 0)  
            return;  
        s++;  
        grid[row][col] = 0;  // 访问过的用0代替
        DFS(grid, s, row + 1, col, rows, cols);  
        DFS(grid, s, row - 1, col, rows, cols);  
        DFS(grid, s, row, col + 1, rows, cols);  
        DFS(grid, s, row, col - 1, rows, cols);  
    }  
};
```

### [547. 省份数量](https://leetcode.cn/problems/number-of-provinces/)
- DFS
```cpp
class Solution {  
public:  
    int findCircleNum(vector<vector<int>>& isConnected) {  
        int rows = isConnected.size();  
        vector<bool> visted(rows, false);  // 已遍历过的城市
        int provinces = 0;  
  
        for (int i = 0; i < rows; i++){  
            if (!visted[i]) {  
                provinces++;  
                DFS(isConnected, visted, i);  // 遍历和该城市相邻的城市
            }  
        }  
        return provinces;  
    }  
  
    void DFS(vector<vector<int>> &isConnected, vector<bool> &visted, int i) { 
        visted[i] = true;  
        for (int j = 0; j < isConnected.size(); j++){  
            if (isConnected[i][j] == 1 && !visted[j])  
                DFS(isConnected, visted, j);  
        }  
    }  
};
```