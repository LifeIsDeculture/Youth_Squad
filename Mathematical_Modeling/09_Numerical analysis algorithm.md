# 数值分析算法

## 1. 方程组求解

### 1.1 算法机理

在实际应用中，许多问题可以转化为求解方程组的形式。常见的方法有：
- **高斯消元法**：用于直接求解线性方程组。
- **雅可比迭代法**和**高斯-赛德尔迭代法**：用于大规模稀疏矩阵的求解。

### 1.2 MATLAB 实现

以下是使用高斯消元法求解线性方程组的示例代码：

```matlab
function x = gauss_elimination(A, b)
    % 高斯消元法求解线性方程组 Ax = b
    n = length(b);
    % 进行消元
    for k = 1:n-1
        for i = k+1:n
            m = A(i,k) / A(k,k);
            A(i,k:n) = A(i,k:n) - m * A(k,k:n);
            b(i) = b(i) - m * b(k);
        end
    end
    % 回代求解
    x = zeros(n, 1);
    for i = n:-1:1
        x(i) = (b(i) - A(i,i+1:n) * x(i+1:n)) / A(i,i);
    end
end
```

## 2. 矩阵运算

### 2.1 算法机理

矩阵运算是数值分析的基础，包括矩阵加法、乘法、求逆等。常用算法有：
- **矩阵乘法**：采用分治法或直接法。
- **矩阵求逆**：可使用高斯消元法或LU分解。

### 2.2 MATLAB 实现

矩阵乘法和求逆的MATLAB代码如下：

```matlab
% 矩阵乘法
function C = matrix_multiplication(A, B)
    % A和B为输入矩阵
    C = zeros(size(A, 1), size(B, 2));
    for i = 1:size(A, 1)
        for j = 1:size(B, 2)
            for k = 1:size(A, 2)
                C(i,j) = C(i,j) + A(i,k) * B(k,j);
            end
        end
    end
end

% 矩阵求逆
function A_inv = matrix_inverse(A)
    % A为输入矩阵，使用高斯消元法求逆
    n = size(A, 1);
    I = eye(n);
    Aug = [A I]; % 扩展矩阵
    for i = 1:n
        Aug(i, :) = Aug(i, :) / Aug(i, i); % 归一化
        for j = 1:n
            if i ~= j
                Aug(j, :) = Aug(j, :) - Aug(i, :) * Aug(j, i);
            end
        end
    end
    A_inv = Aug(:, n+1:end); % 提取逆矩阵
end
```

## 3. 函数积分

### 3.1 算法机理

函数积分常用的方法有：
- **梯形法则**：通过将曲线下的区域分成多个梯形求近似。
- **辛普森法则**：利用抛物线拟合来提高精度。

### 3.2 MATLAB 实现

使用梯形法则进行数值积分的代码如下：

```matlab
function I = trapezoidal_rule(f, a, b, n)
    % 使用梯形法则计算定积分
    h = (b - a) / n; % 步长
    I = 0.5 * (f(a) + f(b)); % 初始化积分值
    for i = 1:n-1
        I = I + f(a + i * h); % 累加中间值
    end
    I = I * h; % 乘以步长
end
```
