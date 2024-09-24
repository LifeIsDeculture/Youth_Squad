# 使用optimtool调出工具箱直接求解非线性规划问题

### 1. 启动 `optimtool`

要启动 `optimtool`，你可以直接在 MATLAB 命令行输入：

```matlab
optimtool
```

这将会打开 MATLAB 的“优化工具”图形用户界面。

### 2. 选择求解器

打开 `optimtool` 后，你需要根据问题类型选择合适的求解器：

- 在 “Solver” 选项中，选择 `fmincon`，这是 MATLAB 中专门用于求解带约束的非线性规划问题的求解器。

`fmincon` 是适合解决非线性规划问题的，因为它可以处理目标函数和约束条件都是非线性的情况。

### 3. 定义目标函数

在 `optimtool` 中，必须定义目标函数。目标函数是优化问题中要最小化或最大化的函数。

假设你有一个非线性目标函数 `fun`，比如：

```matlab
fun = @(x) x(1)^2 + 3*x(2)^2;  % 定义一个简单的二元非线性目标函数
```

你需要在 `Objective function` 框中输入该函数的名字。在这个例子中，假设你将这个函数保存为一个 MATLAB 函数文件 `myObjective.m`，文件内容如下：

```matlab
function f = myObjective(x)
    f = x(1)^2 + 3*x(2)^2;
end
```

那么你可以在 `Objective function` 栏目中输入 `@myObjective`。

### 4. 设置变量初值

在 `optimtool` 的界面上，找到 “Starting point” 栏目。这里你需要设置变量的初始值。假设问题有两个变量，你可以输入一个向量初值，例如 `[0.5, 0.5]`。

### 5. 设置约束条件

非线性规划问题通常还包括约束条件。你可以为问题添加以下几类约束：

#### (1) 边界约束（Bounds）

在 `optimtool` 界面中，找到 “Bounds” 栏目，分别输入每个变量的上下界。例如，如果变量的取值在 `x1 ∈ [0, 1]` 和 `x2 ∈ [-2, 2]` 之间，你可以输入：

- `Lower bounds`：`[0, -2]`
- `Upper bounds`：`[1, 2]`

#### (2) 非线性约束（Nonlinear Constraints）

如果有非线性约束，你需要在 `optimtool` 中指定约束函数。假设非线性约束为 `g(x) = x1^2 + x2^2 - 1 <= 0`，可以将其定义为约束函数文件：

```matlab
function [c, ceq] = myNonlinearConstraint(x)
    c = x(1)^2 + x(2)^2 - 1;  % 非线性不等式约束 g(x) <= 0
    ceq = [];  % 没有非线性等式约束
end
```

在 `Nonlinear constraint function` 框中输入 `@myNonlinearConstraint`。

### 6. 配置算法参数（可选）

你可以通过点击 `Options` 按钮来配置更多的算法参数，比如：

- `Algorithm`: 选择用于求解的具体算法，`fmincon` 提供了不同的算法选项如 `interior-point`, `sqp` 等。
- `TolFun`: 设置目标函数的收敛容忍度。
- `MaxIter`: 最大迭代次数。

### 7. 求解问题

一切设置完成后，点击 `Start` 按钮。`optimtool` 将使用 `fmincon` 求解问题，并在图形界面上显示结果，包括最优解、函数值、是否收敛等。

你也可以在命令行中使用相同的设置运行优化问题：

```matlab
x0 = [0.5, 0.5];  % 初始值
lb = [0, -2];  % 下界
ub = [1, 2];  % 上界
[x_opt, fval] = fmincon(@myObjective, x0, [], [], [], [], lb, ub, @myNonlinearConstraint);
disp(x_opt);
disp(fval);
```

### 8. 查看与分析结果

在 `optimtool` 界面中，求解完成后会显示最优解和一些详细的求解过程信息。如果结果不理想，可能需要调整算法选项或初始条件，然后再次运行求解。
