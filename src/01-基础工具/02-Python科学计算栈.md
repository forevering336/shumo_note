# 第 2 章 · Python 科学计算栈 🟦🟢 ⭐

> 数模编程的主力语言。MATLAB 也能做，但 Python 免费、库全、队友都能装。

---

## 2.1 Python 环境管理

### 确认安装

```bash
python --version   # 应显示 3.9 或更高
pip --version      # 确认 pip 可用
```

### 必装的库

一次装完：

```bash
pip install numpy scipy pandas matplotlib jupyter pulp scikit-learn sympy
```

---

## 2.2 NumPy — 数值计算的基石

### 是什么

Python 的数组运算库。所有科学计算库（SciPy、Pandas、scikit-learn）都建立在它之上。

### 核心操作

```python
import numpy as np

# 创建数组
a = np.array([1, 2, 3, 4, 5])
b = np.zeros((3, 4))         # 3×4 全零矩阵
c = np.ones((2, 3))           # 2×3 全一矩阵
d = np.linspace(0, 10, 100)   # 0到10之间100个等分点
e = np.arange(0, 10, 0.1)     # 0到10，步长0.1

# 矩阵运算
A = np.array([[1, 2], [3, 4]])
B = np.array([[5, 6], [7, 8]])
A @ B          # 矩阵乘法
A * B          # 逐元素乘法
np.linalg.inv(A)   # 矩阵求逆
np.linalg.eig(A)   # 特征值/特征向量
np.linalg.solve(A, b)  # 解 Ax = b

# 常用函数
np.sin(a)      # 三角函数（自动逐元素）
np.exp(a)      # e^x
np.log(a)      # 自然对数
np.sum(a)      # 求和
np.mean(a)     # 均值
np.std(a)      # 标准差
np.max(a)      # 最大值
np.argmax(a)   # 最大值的位置
```

### 数模高频操作

```python
# 生成网格点（画 3D 图常用）
x = np.linspace(-5, 5, 100)
y = np.linspace(-5, 5, 100)
X, Y = np.meshgrid(x, y)
Z = np.sin(np.sqrt(X**2 + Y**2))

# 随机数（模拟/蒙特卡洛）
np.random.seed(42)             # 固定种子，结果可复现
np.random.rand(10)             # [0,1) 均匀分布 10 个
np.random.randn(10)            # 标准正态分布 10 个
np.random.randint(0, 100, 10)  # 0-99 随机整数 10 个
```

---

## 2.3 SciPy — 科学计算工具箱

### 是什么

在 NumPy 之上封装了优化、积分、插值、统计等高级功能。

### 数模最常用的几个模块

```python
from scipy import optimize, integrate, interpolate, linalg, stats

# 1. 优化 — 解线性规划、非线性规划
from scipy.optimize import linprog, minimize, curve_fit
# （详见第 7 章优化模型）

# 2. 数值积分
from scipy.integrate import quad, dblquad
result, error = quad(lambda x: np.exp(-x**2), -np.inf, np.inf)
# result ≈ sqrt(π)

# 3. 插值 — 给离散数据补连续曲线
from scipy.interpolate import interp1d, CubicSpline
f = interp1d(x_data, y_data, kind='cubic')  # 三次样条插值

# 4. 线性代数 — 比 NumPy 更全
from scipy.linalg import lu, qr, svd, null_space

# 5. 统计分布
from scipy.stats import norm, t, chi2, f
norm.ppf(0.975)       # 正态分布 97.5% 分位数 ≈ 1.96
t.ppf(0.975, df=10)   # t 分布
```

---

## 2.4 Pandas — 数据处理

### 是什么

处理表格数据（CSV、Excel）的库，跟 Excel 类似但可以用代码操作。

### 数模中最常用的

```python
import pandas as pd

# 读数据
df = pd.read_csv('data.csv')           # CSV
df = pd.read_excel('data.xlsx')        # Excel

# 看一眼数据
df.head()          # 前 5 行
df.info()          # 每列的数据类型和缺失情况
df.describe()      # 数值列的统计摘要（均值、标准差等）

# 选择/过滤
df['列名']                          # 取一列
df[['列1', '列2']]                  # 取多列
df[df['列名'] > 10]                 # 按条件筛选行
df.loc[5:10, ['列1', '列2']]        # 按位置选

# 缺失值处理
df.dropna()         # 删掉有缺失的行
df.fillna(0)        # 缺失值填 0
df.fillna(df.mean()) # 缺失值填均值

# 分组聚合（类似 Excel 透视表）
df.groupby('类别').mean()
df.groupby('类别').agg(['mean', 'std', 'count'])

# 导出
df.to_csv('output.csv', index=False)
df.to_excel('output.xlsx', index=False)
```

---

## 2.5 Jupyter Notebook

### 是什么

浏览器里写 Python，代码和文字混排，**边写边运行**。数模探索阶段的首选。

### 怎么用

```bash
# 在笔记项目里启动
cd "d:/文件/数模竞赛/学校第一阶段培训"
jupyter lab
```

浏览器会自动打开，新建一个 Notebook，就可以写了。

### 基础操作

| 操作 | 快捷键 |
|------|--------|
| 运行当前单元格 | `Shift + Enter` |
| 新建代码单元格 | `B`（先按 `Esc` 退出编辑模式） |
| 删除单元格 | `D D`（按两次 D） |
| 切换 Markdown 单元格 | `M` |
| 切换代码单元格 | `Y` |

> 💡 Markdown 单元格支持 LaTeX 公式，跟笔记一样用 `$...$` 和 `$$...$$`。

---

> 📖 **参考**：
> - NumPy 官方教程: https://numpy.org/doc/stable/user/quickstart.html
> - Pandas 10 分钟入门: https://pandas.pydata.org/docs/user_guide/10min.html
> - Jupyter 官方文档: https://docs.jupyter.org/
