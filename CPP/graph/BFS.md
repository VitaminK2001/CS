# 搜索网格

## 单源BFS
```cpp
#include <iostream>
#include <vector>
#include <queue>

// 定义坐标结构体，代表 (r, c) 坐标
struct Coord {
    int r;
    int c;
    Coord(int _r, int _c) : r(_r), c(_c) {}
};

// 网格结构的层序遍历
// 从格子 (i, j) 开始遍历
void bfs(std::vector<std::vector<int>>& grid, int i, int j) {
    int numRows = grid.size();
    int numCols = grid[0].size();

    std::queue<Coord> q;
    q.push(Coord(i, j));

    while (!q.empty()) {
        int n = q.size();
        for (int k = 0; k < n; k++) {
            Coord node = q.front();
            q.pop();
            int r = node.r;
            int c = node.c;

            // 上下左右四个方向
            int dr[] = {-1, 1, 0, 0};
            int dc[] = {0, 0, -1, 1};

            for (int dir = 0; dir < 4; dir++) {
                int nr = r + dr[dir];
                int nc = c + dc[dir];

                if (nr >= 0 && nr < numRows && nc >= 0 && nc < numCols && grid[nr][nc] == 0) {
                    grid[nr][nc] = 2;
                    q.push(Coord(nr, nc));
                }
            }
        }
    }
}

int main() {
    // 示例网格
    std::vector<std::vector<int>> grid = {
        {0, 1, 0, 0},
        {0, 0, 0, 1},
        {0, 1, 0, 0},
        {0, 0, 0, 0}
    };

    // 从 (1, 1) 开始进行层序遍历
    bfs(grid, 1, 1);

    // 输出遍历后的网格
    for (const auto& row : grid) {
        for (int val : row) {
            std::cout << val << " ";
        }
        std::cout << std::endl;
    }

    return 0;
}
```

## 多源BFS
```cpp
#include <iostream>
#include <vector>
#include <queue>

// 定义坐标结构体，代表 (r, c) 坐标
struct Coord {
    int r;
    int c;
    Coord(int _r, int _c) : r(_r), c(_c) {}
};

// 判断坐标 (r, c) 是否在网格中
bool inArea(const std::vector<std::vector<int>>& grid, int r, int c) {
    return 0 <= r && r < grid.size() && 0 <= c && c < grid[0].size();
}

int maxDistance(std::vector<std::vector<int>>& grid) {
    int N = grid.size();

    std::queue<Coord> queue;

    // 将所有的陆地格子加入队列
    for (int i = 0; i < N; i++) {
        for (int j = 0; j < N; j++) {
            if (grid[i][j] == 1) {
                queue.push(Coord(i, j));
            }
        }
    }

    // 如果地图上只有陆地或者海洋，返回 -1
    if (queue.empty() || queue.size() == N * N) {
        return -1;
    }

    int moves[4][2] = {
        {-1, 0}, {1, 0}, {0, -1}, {0, 1}
    };

    int distance = -1; // 记录当前遍历的层数（距离）
    while (!queue.empty()) {
        distance++; // 记录最远的格子的距离
        int n = queue.size();
        for (int i = 0; i < n; i++) {
            Coord node = queue.front();
            queue.pop();
            int r = node.r;
            int c = node.c;
            for (int j = 0; j < 4; j++) {
                int r2 = r + moves[j][0];
                int c2 = c + moves[j][1];
                if (inArea(grid, r2, c2) && grid[r2][c2] == 0) {
                    grid[r2][c2] = 2;
                    queue.push(Coord(r2, c2));
                }
            }
        }
    }

    return distance;
}

int main() {
    // 示例网格
    std::vector<std::vector<int>> grid = {
        {1, 0, 1},
        {0, 0, 0},
        {1, 0, 1}
    };

    int result = maxDistance(grid);
    std::cout << "最大距离是：" << result << std::endl;

    return 0;
}

```

## 多源BFS相关题目
https://leetcode.cn/problems/01-matrix/solutions/202012/01ju-zhen-by-leetcode-solution/
求矩阵中所有1到最近0的距离

```cpp
vector<vector<int>> updateMatrix(vector<vector<int>>& mat) {
	
	queue<pair<int, int>> q;
	
	int n = mat.size(), m = mat[0].size();
	
	vector<vector<int>> v(n, vector<int>(m, 0));
	
	vector<vector<int>> dist(n, vector<int>(m, 0));
	
	for(int i = 0; i < n; ++i) {
		for(int j = 0; j < m; ++j) {
			if(mat[i][j] == 0) {
				q.emplace(make_pair(i, j)) ;
				v[i][j] = 1;
			}
		}
	}
	
	int dx[4] = {1,0,-1,0};
	
	int dy[4] = {0,-1,0,1};
	
	while(!q.empty()) {
		auto cur = q.front();
		q.pop();
		for(int d = 0; d < 4; ++d) {
			int nx = cur.first + dx[d];
			int ny = cur.second + dy[d];
			if(0 <= nx && nx < n && 0 <= ny && ny < m && v[nx][ny] == 0) {
				v[nx][ny] = 1;
				dist[nx][ny] = dist[cur.first][cur.second] + 1;
				q.emplace(make_pair(nx, ny));
			}
		}
	}
	
	return dist;
}
```

爆栈可能是因为没有记录那些是遍历过的，遍历回去了
不直接填写棋盘，而是用dist记录当前一步和要走一步之间的联系
关于用make_pair表示坐标  [[make_pair表示坐标]]