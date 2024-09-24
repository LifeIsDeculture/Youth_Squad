# 连续离散化方法

### 1.1 采样法

采样法是通过从连续函数中选取一组离散点来实现离散化。常见的采样方法包括均匀采样和随机采样。

### 1.2 分段函数法

分段函数法通过将连续数据区间划分为多个子区间，并在每个子区间内使用简单的函数（如常数函数或线性函数）进行逼近。

### 1.3 量化法

量化法是将连续值映射到离散值的过程，通常涉及到将连续数据分为若干个区间，并为每个区间分配一个代表值。

## 2. MATLAB实现

下面将分别对以上三种方法进行MATLAB实现。

### 2.1 采样法实现

```matlab
% 定义连续函数
f = @(x) sin(x);

% 采样范围和步长
x = 0:0.1:10; % 均匀采样
y = f(x);    % 计算函数值

% 绘图
figure;
plot(x, y, 'b-', 'LineWidth', 1.5); % 连续函数
hold on;
stem(x, y, 'r', 'MarkerSize', 4); % 离散样本点
title('采样法');
xlabel('x');
ylabel('f(x)');
legend('连续函数', '离散样本');
grid on;
hold off;
```

### 2.2 分段函数法实现

```matlab
% 定义分段函数
f = @(x) (x < 2) .* 1 + (x >= 2 & x < 4) .* 2 + (x >= 4) .* 3;

% 生成连续数据
x = 0:0.1:6;
y = f(x);

% 绘图
figure;
plot(x, y, 'b-', 'LineWidth', 1.5); % 连续函数
hold on;
% 离散化
x_disc = 0:1:6; % 离散点
y_disc = f(x_disc);
stem(x_disc, y_disc, 'r', 'MarkerSize', 4); % 离散样本点
title('分段函数法');
xlabel('x');
ylabel('f(x)');
legend('连续函数', '离散样本');
grid on;
hold off;
```

### 2.3 量化法实现

```matlab
% 定义量化函数
quantize = @(x, step) round(x / step) * step;

% 生成连续数据
x = 0:0.1:10; 
y = sin(x);

% 量化
step = 0.1; % 量化步长
y_quantized = quantize(y, step);

% 绘图
figure;
plot(x, y, 'b-', 'LineWidth', 1.5); % 原始函数
hold on;
stem(x, y_quantized, 'r', 'MarkerSize', 4); % 量化后函数
title('量化法');
xlabel('x');
ylabel('f(x)');
legend('原始函数', '量化函数');
grid on;
hold off;
```
