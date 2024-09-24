# 图论算法在数学建模竞赛中的 MATLAB 实现

## 1. 最短路径算法

### 1.1 Dijkstra 算法

**算法概述**：Dijkstra 算法用于寻找从源点到其他点的最短路径，适用于所有边权非负的图。算法通过逐步扩展已找到的最短路径来找到全局最优解。

**算法步骤**：
1. **初始化**：设定源点到自身的距离为 0，其他点为无穷大，未访问节点列表。
2. **选择节点**：从未访问的节点中选择距离源点最近的节点，并标记为已访问。
3. **更新距离**：对已访问节点的邻居节点更新距离，如果通过当前节点到达邻居的距离更小，则更新该邻居的最短距离。
4. **重复**：重复选择未访问节点和更新距离的步骤，直到所有节点都被访问。

```matlab
function [dist, path] = dijkstra(graph, startNode)
    n = size(graph, 1); % 获取图的节点数
    dist = inf(1, n); % 初始化所有距离为无穷大
    dist(startNode) = 0; % 源点到自身的距离为0
    visited = false(1, n); % 初始化访问状态
    path = cell(1, n); % 初始化路径记录

    for i = 1:n
        % 找到未访问节点中距离源点最近的节点
        [~, u] = min(dist .* ~visited);
        visited(u) = true; % 标记节点u为已访问
        
        for v = 1:n
            % 如果u到v的边存在且v未被访问
            if graph(u, v) > 0 && ~visited(v)
                % 更新v的最短距离和路径
                if dist(u) + graph(u, v) < dist(v)
                    dist(v) = dist(u) + graph(u, v);
                    path{v} = [path{u}, u]; % 记录路径
                end
            end
        end
    end
end
```

### 1.2 Floyd-Warshall 算法

**算法概述**：Floyd-Warshall 算法用于寻找图中任意两点之间的最短路径，适用于边权可以为负的情况。该算法通过动态规划的方法逐步更新路径。

**算法步骤**：
1. **初始化**：将距离矩阵初始化为图的邻接矩阵。
2. **三重循环**：使用三重循环，逐步尝试通过每一个节点 k 更新从节点 i 到节点 j 的距离。如果通过 k 的路径更短，则更新距离。
3. **输出**：最终的距离矩阵包含了所有节点之间的最短路径。

```matlab
function dist = floydWarshall(graph)
    n = size(graph, 1); % 获取节点数
    dist = graph; % 初始化距离矩阵为图的邻接矩阵
    
    for k = 1:n
        for i = 1:n
            for j = 1:n
                % 更新从i到j的最短路径
                if dist(i, j) > dist(i, k) + dist(k, j)
                    dist(i, j) = dist(i, k) + dist(k, j);
                end
            end
        end
    end
end
```

## 2. 网络流算法

### 2.1 Ford-Fulkerson 算法

**算法概述**：Ford-Fulkerson 算法用于解决网络流中的最大流问题。该算法通过不断寻找增广路径来增加流量，直到没有增广路径为止。

**算法步骤**：
1. **初始化**：设置初始流量为 0。
2. **寻找增广路径**：使用广度优先搜索 (BFS) 找到从源点到汇点的增广路径。
3. **更新流量**：找到路径后，计算该路径上的最小剩余容量，更新流量。
4. **重复**：重复上述步骤，直到没有增广路径为止。

```matlab
function maxFlow = fordFulkerson(graph, source, sink)
    maxFlow = 0; % 初始化最大流为0
    n = size(graph, 1); % 获取节点数
    flow = zeros(n); % 初始化流量矩阵为0

    while true
        [path, capacity] = bfs(graph, flow, source, sink); % 寻找增广路径
        if isempty(path) % 如果没有增广路径，结束循环
            break;
        end
        
        minCap = min(capacity); % 找到增广路径中的最小容量
        for i = 1:length(path) - 1
            u = path(i);
            v = path(i + 1);
            flow(u, v) = flow(u, v) + minCap; % 增加流量
            flow(v, u) = flow(v, u) - minCap; % 更新反向流量
        end
        
        maxFlow = maxFlow + minCap; % 更新最大流
    end
end

function [path, capacity] = bfs(graph, flow, source, sink)
    n = size(graph, 1);
    visited = false(1, n); % 初始化访问状态
    queue = [source]; % 使用队列进行广度优先搜索
    parent = zeros(1, n); % 记录增广路径的前驱节点
    capacity = []; % 初始化容量记录
    
    while ~isempty(queue)
        u = queue(1); % 取出队列的第一个元素
        queue(1) = []; % 移除队列的第一个元素
        visited(u) = true; % 标记节点u为已访问
        
        for v = 1:n
            % 如果u到v的边存在且v未被访问
            if ~visited(v) && graph(u, v) > flow(u, v)
                queue(end + 1) = v; % 将节点v加入队列
                parent(v) = u; % 记录前驱节点
                if v == sink % 如果找到汇点，记录路径
                    path = backtrackPath(parent, source, sink);
                    capacity = graph(u, v) - flow(u, v);
                    return;
                end
            end
        end
    end
    
    path = []; % 如果没有增广路径，返回空
    capacity = [];
end

function path = backtrackPath(parent, source, sink)
    path = [];
    while sink ~= source
        path = [sink, path]; % 回溯路径
        sink = parent(sink);
    end
    path = [source, path]; % 添加源点
end
```

## 3. 二分图算法

### 3.1 匈牙利算法

**算法概述**：匈牙利算法用于解决二分图中的最大匹配问题，常用于资源分配、任务调度等场景。该算法通过不断更新匹配，寻找增广路径来增加匹配数量。

**算法步骤**：
1. **初始化**：设置潜在值和匹配数组。
2. **寻找增广路径**：通过 BFS 寻找未匹配的节点，并记录路径。
3. **更新匹配**：如果找到增广路径，则更新匹配关系。
4. **重复**：继续寻找增广路径，直到无法再找到为止。

```matlab
function match = hungarianAlgorithm(costMatrix)
    [n, m] = size(costMatrix); % 获取成本矩阵的大小
    match = zeros(1, m); % 初始化匹配结果
    u = zeros(1, n); % 节点u的潜在值
    v = zeros(1, m); % 节点v的潜在值
    
    for i = 1:n
        links = zeros(1, m); % 初始化链接
        visited = false(1, m); % 初始化访问状态
        clear = false(1, m); % 初始化清除状态
        j0 = 0; % 当前节点
        matchCount = 0; % 当前匹配数量
        
        while true
            j1 = 0; % 临时节点
            delta = inf; % 初始化增益
            visited(1, j0 + 1) = true; % 标记当前节点为已访问
            
            while true
                j0 = j1; % 更新当前节点
                i0 = match(j0 + 1); % 获取前驱节点
                j1 = 0; % 重置临时节点
                
                for j = 1:m
                    if visited(j) % 跳过已访问节点
                        continue;
                    end
                    cur = costMatrix(i0 + 1, j) - u(i0 + 1) - v(j); % 计算当前成本
                    if cur < delta