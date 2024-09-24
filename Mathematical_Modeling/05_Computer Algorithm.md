# 计算机算法：动态规划、回溯搜索、分治算法、分支定界

## 1. 动态规划

### 1.1 算法机理

动态规划通过将问题分解为子问题，存储已解决的子问题的结果，从而避免重复计算。常用于最短路径、最优子结构问题。

### 1.2 MATLAB 实现示例

以下是使用动态规划解决最短路径问题的 MATLAB 代码：

```matlab
function dist = dijkstra(graph, startNode)
    n = size(graph, 1);
    dist = inf(1, n);
    dist(startNode) = 0;
    visited = false(1, n);
    
    for i = 1:n
        % 找到未访问节点中距离最小的节点
        [~, u] = min(dist + visited * max(dist));
        visited(u) = true;
        
        % 更新邻居节点的距离
        for v = 1:n
            if graph(u, v) > 0 && ~visited(v)
                dist(v) = min(dist(v), dist(u) + graph(u, v));
            end
        end
    end
end
```

## 2. 回溯搜索

### 2.1 算法机理

回溯搜索是一种通过构建解空间树逐步尝试解决问题的方法，适合解决组合优化问题，例如图的着色、旅行商问题等。

### 2.2 MATLAB 实现示例

以下是使用回溯算法解决 N 皇后问题的代码：

```matlab
function solutions = nQueens(n)
    solutions = [];
    board = zeros(n);
    solve(board, 1, n, solutions);
end

function solve(board, row, n, solutions)
    if row > n
        solutions = [solutions; board(:)'];
        return;
    end
    for col = 1:n
        if isSafe(board, row, col, n)
            board(row, col) = 1;
            solve(board, row + 1, n, solutions);
            board(row, col) = 0; % 回溯
        end
    end
end

function safe = isSafe(board, row, col, n)
    safe = all(board(1:row-1, col) == 0) && ... % 列检查
           all(diag(board(1:row-1, :), col-row) == 0) && ... % 主对角线检查
           all(diag(flipud(board(1:row-1, :)), row-col) == 0); % 副对角线检查
end
```

## 3. 分治算法

### 3.1 算法机理

分治算法将问题分解为多个相似的子问题，分别解决后再合并结果。常用于排序、查找等问题。

### 3.2 MATLAB 实现示例

以下是快速排序的 MATLAB 实现：

```matlab
function sortedArray = quickSort(array)
    if length(array) <= 1
        sortedArray = array;
    else
        pivot = array(1);
        left = array(array < pivot);
        right = array(array > pivot);
        sortedArray = [quickSort(left), pivot, quickSort(right)];
    end
end
```

## 4. 分支定界

### 4.1 算法机理

分支定界是一种优化算法，通过创建解空间树并剪枝来寻找最优解，适用于整数线性规划、背包问题等。

### 4.2 MATLAB 实现示例

以下是一个简单的背包问题的分支定界实现：

```matlab
function maxValue = knapsack(values, weights, capacity)
    n = length(values);
    maxValue = 0;
    bestValue = 0;
    knapsackBranch(0, 0, 0);
    
    function knapsackBranch(i, currentWeight, currentValue)
        if currentWeight <= capacity
            maxValue = max(maxValue, currentValue);
        end
        if i >= n || currentWeight >= capacity
            return;
        end
        % 包含第 i 个物品
        knapsackBranch(i + 1, currentWeight + weights(i + 1), currentValue + values(i + 1));
        % 不包含第 i 个物品
        knapsackBranch(i + 1, currentWeight, currentValue);
    end
end
```
