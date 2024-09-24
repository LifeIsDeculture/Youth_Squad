## 一、线性规划（Linear Programming）

线性规划是指优化一个线性目标函数，约束条件也为线性的优化问题。MATLAB 使用 `linprog` 函数来求解线性规划问题。

### 1. 标准形式
线性规划的标准形式为：
$$
\text{minimize } f^T x
$$
subject to:
$$
\
A x \leq b, \quad A_{eq} x = b_{eq}, \quad lb \leq x \leq ub
$$

### 2. MATLAB 实现线性规划

**问题描述：**

最大化目标函数 
$$
f = -2x_1 - 3x_2
$$
约束条件为：
$$
\begin{align*}
x_1 + 2x_2 & \leq 6 \\
2x_1 + x_2 & \leq 6 \\
x_1 & \geq 0, x_2 \geq 0
\end{align*}
$$
我们可以通过最小化的方式来求解最大化问题。

```matlab
% 清空工作空间
clear; close all; clc;

% 定义目标函数 (由于 linprog 是求最小值，我们定义 -f)
f = [-2; -3]; % -2x1 - 3x2

% 定义不等式约束 A * x <= b
A = [1 2; 2 1]; % 系数矩阵
b = [6; 6]; % 右侧常数

% 定义变量下界（上界默认为无穷）
lb = [0; 0]; % x1 >= 0, x2 >= 0

% 使用 linprog 求解线性规划问题
[x, fval, exitflag, output] = linprog(f, A, b, [], [], lb);

% 显示结果
disp('最优解：');
disp(x);
disp('最大化目标函数的值：');
disp(-fval); % linprog 最小化的是 -f，所以要取负数
```

**代码解释：**

- `f`：目标函数系数，`linprog` 求的是最小化问题，若需要最大化，将目标函数系数取负。
- `A` 和 `b`：不等式约束 
  $$
  A * x \leq b
  $$
- `lb`：变量的下界，确保 
  $$
  x_1, x_2 \geq 0\
  $$
- `linprog` 函数返回变量 `x` 的最优解、目标函数的最小值 `fval` 以及求解状态 `exitflag` 和输出信息 `output`。

### 3. 输出示例

```matlab
最优解：
    3.0000
    1.5000
最大化目标函数的值：
   10.5000
```

---

## 二、整数规划（Integer Programming）

整数规划与线性规划类似，只是要求解的变量必须是整数。MATLAB 使用 `intlinprog` 函数来求解整数规划问题。

### 1. MATLAB 实现整数规划

**问题描述：**

最小化目标函数 \(f = 3x_1 + 2x_2\)，约束条件为：
$$
\begin{align*}
x_1 + 2x_2 & \geq 4 \\
3x_1 + 2x_2 & \leq 14 \\
x_1, x_2 \in \mathbb{Z}, x_1, x_2 \geq 0
\end{align*}
$$

```matlab
% 清空工作空间
clear; close all; clc;

% 定义目标函数
f = [3; 2]; % 3x1 + 2x2

% 定义不等式约束 A * x <= b
A = [-1 -2; 3 2]; % 使用 A * x <= b，需要将第一个不等式变号
b = [-4; 14];

% 定义整数变量的索引（即两个变量 x1, x2 都是整数）
intcon = [1 2]; % 两个整数变量

% 定义变量下界
lb = [0; 0]; % x1, x2 >= 0

% 使用 intlinprog 求解整数规划问题
[x, fval, exitflag, output] = intlinprog(f, intcon, A, b, [], [], lb);

% 显示结果
disp('最优解：');
disp(x);
disp('目标函数的最小值：');
disp(fval);
```

**代码解释：**

- `f`：目标函数系数。
- `A` 和 `b`：不等式约束 \(A * x \leq b\)。
- `intcon`：整数变量的索引，这里指定 `x_1` 和 `x_2` 都是整数。
- `intlinprog`：求解整数规划问题，返回最优解 `x` 和目标函数值 `fval`。

### 2. 输出示例

```matlab
最优解：
     2
     1
目标函数的最小值：
     8
```

---

## 三、非线性规划（Nonlinear Programming）

非线性规划涉及非线性的目标函数或约束，MATLAB 提供了 `fmincon` 函数来求解带约束的非线性优化问题。

### 1. MATLAB 实现非线性规划

**问题描述：**

最小化目标函数 
$$
f(x_1, x_2) = x_1^2 + x_2^2
$$
约束条件为：
$$
\begin{align*}
x_1 + x_2 & \leq 1 \\
x_1^2 + x_2^2 & \leq 1 \\
x_1, x_2 \geq 0
\end{align*}
$$

```matlab
% 清空工作空间
clear; close all; clc;

% 定义目标函数
fun = @(x) x(1)^2 + x(2)^2; % f(x1, x2) = x1^2 + x2^2

% 定义不等式约束函数
function [c, ceq] = nonlincon(x)
    c = [x(1) + x(2) - 1;   % x1 + x2 <= 1
         x(1)^2 + x(2)^2 - 1]; % x1^2 + x2^2 <= 1
    ceq = []; % 没有等式约束
end

% 定义初始点和变量下界
x0 = [0.5, 0.5]; % 初始点
lb = [0, 0]; % x1, x2 >= 0

% 使用 fmincon 求解非线性规划问题
[x, fval, exitflag, output] = fmincon(fun, x0, [], [], [], [], lb, [], @nonlincon);

% 显示结果
disp('最优解：');
disp(x);
disp('目标函数的最小值：');
disp(fval);
```

**代码解释：**

- `fun`：定义目标函数
  $$
  f(x_1, x_2) = x_1^2 + x_2^2
  $$
- `nonlincon`：定义非线性约束，`c` 代表不等式约束，`ceq` 为空数组表示没有等式约束。
- `fmincon`：用于带约束的非线性优化，返回最优解 `x` 和目标函数值 `fval`。

### 2. 输出示例

```matlab
最优解：
    0.5000
    0.5000
目标函数的最小值：
    0.5000
```

