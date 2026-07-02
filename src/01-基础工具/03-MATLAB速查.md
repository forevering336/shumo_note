# 第 3 章 · MATLAB 速查 

> 学校里 MATLAB 用得比 Python 多。如果你的队伍建模手只会 MATLAB，编程手需要能读懂 MATLAB 代码，或者直接用 MATLAB 写。

---

## 3.1 MATLAB vs Python 对照

大部分模型两种语言都能写，选你们队伍最熟的：

| | MATLAB | Python |
|------|--------|--------|
| 优势 | 矩阵运算直观、工具箱全（优化/偏微分/系统辨识）、学校有正版 | 免费、库多、机器学习生态好 |
| 劣势 | 付费（学校买了就无所谓）、部署麻烦 | 矩阵写法不如 MATLAB 自然 |
| 选哪个 | 建模手习惯 MATLAB | 编程手习惯 Python |

**结论**：两人各自用擅长的，论文手负责整合代码逻辑到论文里。不用统一语言。

---

## 3.2 矩阵和数组（MATLAB 核心语法）

```matlab
% 创建矩阵
A = [1 2 3; 4 5 6; 7 8 9]; % 3×3 矩阵，分号换行
b = [1; 2; 3]; % 列向量
c = 1:0.1:10; % 1 到 10，步长 0.1
d = linspace(0, 10, 100); % 0 到 10，100 个点（跟上面类似）

% 矩阵运算
A * b % 矩阵乘法
A .* B % 逐元素乘法（加个点）
A ./ B % 逐元素除法
A' % 转置
inv(A) % 求逆
eig(A) % 特征值
det(A) % 行列式
rank(A) % 秩

% 解线性方程组 Ax = b
x = A \ b; % 反斜杠，MATLAB 最经典的语法

% 全零/全一矩阵
zeros(3, 4) % 3×4 零矩阵
ones(2, 3) % 2×3 一矩阵
eye(4) % 4×4 单位矩阵
```

---

## 3.3 常用数学函数

```matlab
sin(x), cos(x), tan(x) % 三角函数
exp(x), log(x), log10(x) % 指数和对数
abs(x) % 绝对值
sqrt(x) % 开方
round(x), floor(x), ceil(x) % 取整
mod(a, b) % 取余
sum(x), mean(x), std(x) % 统计
max(x), min(x) % 最值
```

---

## 3.4 绘图（数模最常用）

```matlab
% 二维线图
x = linspace(0, 2*pi, 100);
y = sin(x);
plot(x, y, 'LineWidth', 1.5);
xlabel('x');
ylabel('sin(x)');
title('正弦函数');
grid on;

% 多条曲线
plot(x, sin(x), 'b-', x, cos(x), 'r--', 'LineWidth', 1.5);
legend('sin', 'cos');

% 三维曲面
[X, Y] = meshgrid(-5:0.1:5, -5:0.1:5);
Z = sin(sqrt(X.^2 + Y.^2));
surf(X, Y, Z);
xlabel('x'); ylabel('y'); zlabel('z');
title('3D 曲面');

% 等高线图
contour(X, Y, Z, 20); % 20 条等高线
contourf(X, Y, Z, 20); % 填充等高线
```

---

## 3.5 优化工具箱（数模核心）

```matlab
% 线性规划: linprog
% min c'*x s.t. A*x <= b, Aeq*x = beq, lb <= x <= ub
f = [-1; -2]; % 目标系数（负号求最大）
A = [2 1; 1 2];
b = [6; 6];
lb = [0; 0];
[x, fval] = linprog(f, A, b, [], [], lb, []);

% 非线性规划: fmincon
% min fun(x) s.t. constraints
fun = @(x) (x(1)-1)^2 + (x(2)-2)^2; % 匿名函数
x0 = [0, 0];
A = []; b = []; % 无线性约束
Aeq = []; beq = [];
lb = [0, 0]; ub = [];
nonlcon = @(x) deal([], x(1) + x(2) - 3); % 非线性约束 x1+x2>=3
[x, fval] = fmincon(fun, x0, A, b, Aeq, beq, lb, ub, nonlcon);

% 整数规划: intlinprog
% 有 intcon 参数指定哪些变量是整数
```

---

## 3.6 读数据 & 出结果

```matlab
% 读取数据
data = readmatrix('data.csv'); % CSV
data = readtable('data.xlsx'); % Excel（保留表头）
T = readtable('data.xlsx', 'Sheet', 2); % 指定第 2 个工作表

% 导出结果
writematrix(result, 'result.csv');
writetable(T, 'result.xlsx');
```

---

## 3.7 脚本与函数

```matlab
% 脚本：把所有命令写在一个 .m 文件里，直接运行
% myscript.m
x = linspace(0, 10, 100);
y = myfunc(x); % 调用下面定义的函数
plot(x, y);

% 函数：单独的 .m 文件，文件名 = 函数名
% myfunc.m
function y = myfunc(x)
 y = x.^2 .* exp(-x);
end
```

---

> 📖 **参考**：MATLAB 官方文档 https://www.mathworks.com/help/matlab/
