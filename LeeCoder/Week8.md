---
last_reviewed: 2025-07-28
review_count: "1"
---

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

### [994. 腐烂的橘子](https://leetcode.cn/problems/rotting-oranges/)
- BFS
```cpp
class Solution {  
public:  
    int orangesRotting(vector<vector<int>>& grid) {  
        queue<pair<int, int>> rotting;  
        int fresh = 0;  
        int cols = grid[0].size();  
        int res = 0;  
        int rows = grid.size();  
        for (int i = 0; i < rows; i++) {    // 查找所有腐烂橘子
            for (int j = 0; j < cols; j++) {  
                if (grid[i][j] == 2)  
                    rotting.push({i, j});  
                else if (grid[i][j] == 1)  
                    fresh++;      
            }  
        }  
        // BFS + 队列  每一轮就代表一分钟
        vector<pair<int, int>> direc = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};  
        while (!rotting.empty() && fresh > 0) {  
            int round = rotting.size();  
            for (int i = 0; i < round; i++) {  
                pair<int, int> t = rotting.front();  
                rotting.pop();  
                for (auto [ro, co] : direc) {  
                    int row = t.first + ro;  
                    int col = t.second + co;  
                    if (row >= 0 && row < rows && col >= 0 && col < cols && grid[row][col] == 1) {  
                        grid[row][col] = 2;  
                        fresh--;  
                        rotting.push({row, col});  
                    }  
                }  
            }  
            if (!rotting.empty())  
               res++;  
        }  
        return fresh == 0 ? res : -1;  
    }  
};
```
>[!note]
>位移向量{{1, 0}, {-1, 0}, {0, 1}, {0, -1}}
> 
>BFS（广度优先搜索）类题目通常具有以下 典型特征：
>- 最短路径，
>- 每一层代表“一步/一轮/一秒”，一般带有“轮数”概念，每一层扩展都需要统计次数。
>- BFS天然适合模拟“从一个点向四周蔓延”的过程。

### [207. 课程表](https://leetcode.cn/problems/course-schedule/)
- 拓扑排序
```cpp
class Solution {  
public:  
    bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {  
        vector<vector<int>> graph(numCourses);  // 构建邻接表
        vector<int> inDegree(numCourses, 0);   // 入度数组
  
        for (auto pre : prerequisites) {  
            graph[pre[1]].push_back(pre[0]);  
            inDegree[pre[0]]++;       // 统计每个点的入度 
        }  
  
        queue<int> que;  
        for (int i = 0; i < numCourses; i++) {  
            if (inDegree[i] == 0)    // 入度为0 加入队列
                que.push(i);  
        }  
        int count = 0;  
        while (!que.empty()) {  
            int c = que.front();  
            que.pop();  
            count++;  
  
            for (auto adjency : graph[c]) {  
                if (--inDegree[adjency] == 0)    // 更新入度
                    que.push(adjency);  
            }  
        }  
        return count == numCourses;  
    }  
};

// 拓扑排序模板

vector<int> topologicalSort(int n, const vector<vector<int>>& edges) {
    vector<vector<int>> graph(n);
    vector<int> indegree(n, 0);

    // 构建图和入度数组
    for (auto& edge : edges) {
        int u = edge[0], v = edge[1];
        graph[u].push_back(v);
        indegree[v]++;
    }

    queue<int> q;
    vector<int> res;

    // 入度为0的节点入队
    for (int i = 0; i < n; ++i)
        if (indegree[i] == 0)
            q.push(i);

    // BFS
    while (!q.empty()) {
        int u = q.front(); q.pop();
        res.push_back(u);
        for (int v : graph[u]) {
            if (--indegree[v] == 0)
                q.push(v);
        }
    }

    // 如果有环
    if (res.size() != n) return {};

    return res;
}
```
>[!note]
>拓扑排序-> 任务依赖，先后问题，判断图中是否有环


