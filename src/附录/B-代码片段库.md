# 附录 B · 代码片段库

这里保留比赛中经常重复使用的小段代码。使用前应根据题目修改字段名、单位和参数，不要把示例直接复制到正式结果中。

---

## 1 统一随机种子

```python
import os
import random
import numpy as np

SEED = 42
os.environ["PYTHONHASHSEED"] = str(SEED)
random.seed(SEED)
np.random.seed(SEED)
```

部分并行库和 GPU 算法仍可能存在微小非确定性，因此还应记录软件版本与硬件环境。

---

## 2 读取数据后的基本检查

```python
from pathlib import Path
import pandas as pd

data_path = Path("data_raw") / "附件1.xlsx"
df = pd.read_excel(data_path)

print("形状:", df.shape)
print("字段:", df.columns.tolist())
print("缺失值:")
print(df.isna().sum().sort_values(ascending=False).head(20))
print("重复行:", df.duplicated().sum())
print(df.describe(include="all").T)
```

建议把检查结果保存到日志中。读取后立即确认行数、字段和单位，可以避免后续在错误数据上反复调模型。

---

## 3 正向与负向指标标准化

```python
import numpy as np

def minmax_positive(x):
    """数值越大越好，映射到 [0, 1]。"""
    x = np.asarray(x, dtype=float)
    span = x.max() - x.min()
    return np.ones_like(x) if span == 0 else (x - x.min()) / span

def minmax_negative(x):
    """数值越小越好，映射到 [0, 1]。"""
    x = np.asarray(x, dtype=float)
    span = x.max() - x.min()
    return np.ones_like(x) if span == 0 else (x.max() - x) / span
```

常数列没有区分能力。示例返回全 1，正式评价时也可以直接删除该指标，但必须说明处理方法。

---

## 4 回归指标

```python
import numpy as np

def regression_metrics(y_true, y_pred):
    y_true = np.asarray(y_true, dtype=float)
    y_pred = np.asarray(y_pred, dtype=float)
    error = y_true - y_pred

    mae = np.mean(np.abs(error))
    rmse = np.sqrt(np.mean(error ** 2))
    ss_res = np.sum(error ** 2)
    ss_tot = np.sum((y_true - y_true.mean()) ** 2)
    r2 = np.nan if ss_tot == 0 else 1 - ss_res / ss_tot
    return {"MAE": mae, "RMSE": rmse, "R2": r2}
```

当真实值可能接近 0 时，不宜直接使用 MAPE。时间序列还应与“上一期值”等简单基线比较。

---

## 5 分层与时间顺序划分

### 分类任务

```python
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(
    X,
    y,
    test_size=0.2,
    random_state=42,
    stratify=y,
)
```

### 时间序列

```python
split = int(len(df) * 0.8)
train = df.iloc[:split].copy()
test = df.iloc[split:].copy()
```

时间序列不能随机打乱。标准化、缺失值填补和特征筛选只能在训练集上拟合。

---

## 6 线性规划

`scipy.optimize.linprog` 默认求最小值。最大化问题需要把目标系数取负。

```python
import numpy as np
from scipy.optimize import linprog

# max 40*x_A + 50*x_B
c = -np.array([40.0, 50.0])
A_ub = np.array([
    [2.0, 4.0],
    [3.0, 2.0],
])
b_ub = np.array([100.0, 120.0])

result = linprog(
    c,
    A_ub=A_ub,
    b_ub=b_ub,
    bounds=[(0, None), (0, None)],
    method="highs",
)

if not result.success:
    raise RuntimeError(result.message)

print("方案:", result.x)
print("最大利润:", -result.fun)
print("最大约束违反量:", np.maximum(A_ub @ result.x - b_ub, 0).max())
```

若产量必须为整数，应使用整数规划求解器，不能把连续解直接作为最终答案。

---

## 7 常微分方程

```python
import numpy as np
from scipy.integrate import solve_ivp

def logistic(t, y, r, K):
    return r * y * (1 - y / K)

t_eval = np.linspace(0, 20, 201)
sol = solve_ivp(
    logistic,
    t_span=(t_eval[0], t_eval[-1]),
    y0=[10.0],
    t_eval=t_eval,
    args=(0.4, 100.0),
    rtol=1e-7,
    atol=1e-9,
)

if not sol.success:
    raise RuntimeError(sol.message)
```

改变容差后重新求解，若主要结论明显变化，应检查刚性、参数敏感性或模型形式。

---

## 8 图模型

```python
import networkx as nx

G = nx.Graph()
G.add_weighted_edges_from([
    ("A", "B", 4),
    ("A", "C", 2),
    ("C", "E", 3),
    ("E", "D", 4),
    ("B", "D", 10),
])

path = nx.shortest_path(G, "A", "D", weight="weight")
distance = nx.shortest_path_length(G, "A", "D", weight="weight")
print(path, distance)
```

建立图时应先确认有向/无向、边权单位和重复边处理规则。

---

## 9 保存图片

```python
from pathlib import Path
import matplotlib.pyplot as plt

output_dir = Path("output")
output_dir.mkdir(exist_ok=True)

fig, ax = plt.subplots(figsize=(7, 4.5))
ax.plot(x, y, linewidth=1.8)
ax.set_xlabel("时间 / d")
ax.set_ylabel("数量 / 个")
ax.grid(alpha=0.25)
fig.tight_layout()
fig.savefig(output_dir / "结果曲线.png", dpi=300, bbox_inches="tight")
plt.close(fig)
```

论文图应保留生成脚本，不要只保存截图。

---

## 10 环境信息

```python
import platform
import numpy
import pandas
import scipy
import sklearn

print("Python:", platform.python_version())
print("NumPy:", numpy.__version__)
print("pandas:", pandas.__version__)
print("SciPy:", scipy.__version__)
print("scikit-learn:", sklearn.__version__)
```

真题复现和团队协作时，把这段输出放进运行日志，能减少版本不同造成的问题。
