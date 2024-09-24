## 一、数据拟合（Data Fitting）

数据拟合是通过构建数学模型来逼近数据集，以便理解数据的趋势或进行预测。

### 1. 多项式拟合（`polyfit`）

**示例：**

假设我们有一组实验数据，需要用二次多项式进行拟合。

```matlab
% 清空工作空间
clear; close all; clc;

% 定义数据点
x = [1, 2, 3, 4, 5];
y = [2.2, 4.8, 7.1, 8.9, 11.2];

% 使用二次多项式进行拟合
n = 2; % 多项式阶数
p = polyfit(x, y, n); % 返回多项式系数

% 显示拟合的多项式系数
disp('拟合的多项式系数：');
disp(p);

% 生成拟合曲线的数据点
x_fit = linspace(min(x), max(x), 100);
y_fit = polyval(p, x_fit);

% 绘制原始数据点和拟合曲线
figure;
plot(x, y, 'ro', 'MarkerSize', 8, 'DisplayName', '原始数据');
hold on;
plot(x_fit, y_fit, 'b-', 'LineWidth', 2, 'DisplayName', '拟合曲线');
xlabel('x');
ylabel('y');
title('多项式拟合');
legend show;
grid on;
```

**代码解释：**

- `polyfit(x, y, n)`：用于计算 n 阶多项式拟合，返回多项式系数。
- `polyval(p, x_fit)`：根据多项式系数计算对应的 y 值。
- 使用 `plot` 函数绘制原始数据和拟合曲线。

### 2. 非线性曲线拟合（`fit` 函数）

**示例：**

使用 MATLAB 的 Curve Fitting Toolbox 进行非线性拟合，例如拟合指数函数。

```matlab
matlab% 清空工作空间
clear; close all; clc;

% 定义数据点
x = (0:0.5:5)';
y = 2 * exp(0.8 * x) + randn(size(x)); % 加入一些随机噪声

% 定义拟合类型
fitType = fittype('a*exp(b*x)', 'independent', 'x', 'coefficients', {'a', 'b'});

% 进行拟合
[fitResult, gof] = fit(x, y, fitType);

% 显示拟合结果
disp('拟合参数：');
disp(fitResult);

% 绘制拟合结果
figure;
plot(fitResult, x, y);
xlabel('x');
ylabel('y');
title('非线性曲线拟合');
legend('数据点', '拟合曲线');
grid on;
```

**代码解释：**

- `fittype`：定义自定义的拟合函数类型。
- `fit`：进行数据拟合，返回拟合结果和拟合优度。
- `gof` 包含了 `R-square` 等评价指标。

------

## 二、参数估计（Parameter Estimation）

参数估计用于根据数据来估计模型中的未知参数。

### 1. 最小二乘曲线拟合（`lsqcurvefit`）

**示例：**

使用最小二乘方法估计非线性模型的参数。

```matlab
% 清空工作空间
clear; close all; clc;

% 定义模型函数
modelFun = @(p, x) p(1) * exp(p(2) * x);

% 生成模拟数据
xData = (0:0.5:5)';
actualParams = [2, 0.8]; % 实际参数
yData = modelFun(actualParams, xData) + 0.5 * randn(size(xData)); % 加入噪声

% 初始参数猜测
initialGuess = [1, 0.5];

% 使用 lsqcurvefit 进行参数估计
% 需要优化工具箱
options = optimoptions('lsqcurvefit', 'Display', 'iter');
[estimatedParams, resnorm, residual, exitflag] = lsqcurvefit(modelFun, initialGuess, xData, yData, [], [], options);

% 显示估计的参数
disp('估计的参数：');
disp(estimatedParams);

% 绘制拟合结果
yFit = modelFun(estimatedParams, xData);
figure;
plot(xData, yData, 'ro', 'DisplayName', '数据点');
hold on;
plot(xData, yFit, 'b-', 'LineWidth', 2, 'DisplayName', '拟合曲线');
xlabel('x');
ylabel('y');
title('最小二乘参数估计');
legend show;
grid on;
```

**代码解释：**

- `modelFun`：定义模型函数，其中 `p` 是待估计的参数向量。
- `lsqcurvefit`：求解非线性最小二乘问题，估计参数值。
- `options`：设置优化选项，如显示迭代过程。

### 2. 非线性回归（`nlinfit`）

**示例：**

使用统计工具箱的 `nlinfit` 进行非线性回归。

```matlab
% 清空工作空间
clear; close all; clc;

% 定义模型函数
modelFun = @(p, x) p(1) ./ (1 + exp(-p(2) * (x - p(3))));

% 生成模拟数据
xData = (0:0.5:10)';
actualParams = [10, 1, 5]; % 实际参数
yData = modelFun(actualParams, xData) + randn(size(xData)); % 加入噪声

% 初始参数猜测
initialGuess = [8, 0.5, 4];

% 使用 nlinfit 进行参数估计
[estimatedParams, R, J, CovB, MSE] = nlinfit(xData, yData, modelFun, initialGuess);

% 显示估计的参数
disp('估计的参数：');
disp(estimatedParams);

% 绘制拟合结果
yFit = modelFun(estimatedParams, xData);
figure;
plot(xData, yData, 'ro', 'DisplayName', '数据点');
hold on;
plot(xData, yFit, 'g-', 'LineWidth', 2, 'DisplayName', '拟合曲线');
xlabel('x');
ylabel('y');
title('非线性回归参数估计');
legend show;
grid on;
```

**代码解释：**

- `nlinfit`：用于非线性回归的函数，返回估计参数和统计信息。
- `R`、`J`、`CovB`、`MSE`：分别为残差、Jacobian 矩阵、参数协方差和均方误差。

------

## 三、插值（Interpolation）

插值用于根据已知数据点估计未知点的值，常用于数据平滑和放大。

### 1. 一维插值（`interp1`）

**示例：**

使用 `interp1` 进行一维线性和样条插值。

```matlab
% 清空工作空间
clear; close all; clc;

% 已知数据点
x = 0:2:10;
y = sin(x);

% 新的数据点（需要插值的点）
xq = 0:0.1:10;

% 线性插值
y_linear = interp1(x, y, xq, 'linear');

% 三次样条插值
y_spline = interp1(x, y, xq, 'spline');

% 绘制结果
figure;
plot(x, y, 'ro', 'MarkerSize', 8, 'DisplayName', '已知数据点');
hold on;
plot(xq, y_linear, 'b-', 'LineWidth', 2, 'DisplayName', '线性插值');
plot(xq, y_spline, 'g--', 'LineWidth', 2, 'DisplayName', '样条插值');
xlabel('x');
ylabel('y');
title('一维插值示例');
legend show;
grid on;
```

**代码解释：**

- `interp1`：进行一维插值，第一个参数是已知的 x，第二个参数是已知的 y，第三个参数是查询点，第四个参数指定插值方法。
- 插值方法包括 `'linear'`（线性）、`'spline'`（样条）、`'nearest'`（最近邻）等。

### 2. 二维插值（`interp2`）

**示例：**

对二维数据进行插值。

```matlab
% 清空工作空间
clear; close all; clc;

% 定义网格数据
[X, Y] = meshgrid(-5:1:5, -5:1:5);
Z = X.^2 + Y.^2;

% 定义查询点
[Xq, Yq] = meshgrid(-5:0.1:5, -5:0.1:5);

% 进行插值
Zq = interp2(X, Y, Z, Xq, Yq, 'spline');

% 绘制结果
figure;
surf(Xq, Yq, Zq);
xlabel('X');
ylabel('Y');
zlabel('Z');
title('二维插值示例');
shading interp;
```

**代码解释：**

- `meshgrid`：生成网格坐标，用于二维函数的绘制和计算。
- `interp2`：对二维数据进行插值，参数与 `interp1` 类似，但针对二维数据。