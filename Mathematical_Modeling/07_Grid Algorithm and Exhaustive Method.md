# 网格算法与穷举法

## 1. 网格算法

### 1.1 机理

网格算法通过在特定的参数空间上建立一个网格，对每个网格点进行评估，从而寻找最优解。该方法适用于参数较少的情况。

### 1.2 MATLAB 实现

```matlab
% 网格算法示例
% 定义目标函数
objective_function = @(x) (x(1)-3)^2 + (x(2)-2)^2; % 示例目标函数

% 定义搜索范围
x1_range = linspace(0, 6, 100); % x1 范围
x2_range = linspace(0, 6, 100); % x2 范围
best_value = inf; % 初始化最优值
best_point = [0, 0]; % 初始化最优点

% 网格搜索
for x1 = x1_range
    for x2 = x2_range
        current_value = objective_function([x1, x2]);
        if current_value < best_value
            best_value = current_value;
            best_point = [x1, x2];
        end
    end
end

disp(['最优点: ', num2str(best_point)]);
disp(['最优值: ', num2str(best_value)]);
```

## 2. 穷举法

### 2.1 机理

穷举法通过列出所有可能的解，评估每个解以确定最优解。此方法确保不会错过任何潜在解，但对时间和计算资源的需求极高。

### 2.2 MATLAB 实现

```matlab
% 穷举法示例
% 定义目标函数
objective_function = @(x) (x(1)-3)^2 + (x(2)-2)^2; % 示例目标函数

% 定义搜索范围
x1_values = [0, 1, 2, 3, 4, 5, 6]; % x1 可能值
x2_values = [0, 1, 2, 3, 4, 5, 6]; % x2 可能值
best_value = inf; % 初始化最优值
best_point = [0, 0]; % 初始化最优点

% 穷举搜索
for x1 = x1_values
    for x2 = x2_values
        current_value = objective_function([x1, x2]);
        if current_value < best_value
            best_value = current_value;
            best_point = [x1, x2];
        end
    end
end

disp(['最优点: ', num2str(best_point)]);
disp(['最优值: ', num2str(best_value)]);
```
