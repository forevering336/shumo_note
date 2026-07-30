# 数据包络分析-DEA

数据包络分析（Data Envelopment Analysis，DEA）是一种用于评估决策单元（Decision Making Units，DMU）相对效率的非参数方法。DEA通过线性规划对多输入、多输出的生产或服务单元进行效率分析，无需预先设定生产函数形式。

## 1 DEA的基本思想

DEA将每个评价对象视为一个DMU，按输入和输出构建效率评价模型。通过比较各DMU的投入与产出关系，确定相对效率前沿面，并将评价对象分为有效前沿上的有效DMU与效率低下的非有效DMU。

## 2 DEA模型类型

### 2.1 CCR模型

CCR模型由Charnes、Cooper和Rhodes提出，假定规模报酬不变（Constant Returns to Scale，CRS）。模型形式为：

- 目标：max θ
- 约束：∑_{j=1}^n y_{rj} λ_j ≥ y_{rk} for r = 1,...,s
- ∑_{j=1}^n x_{ij} λ_j ≤ θ x_{ik} for i = 1,...,m
- λ_j ≥ 0

其中x_{ij}、y_{rj}分别为DMU j的第i个输入和第r个输出，θ为效率值。

### 2.2 BCC模型

BCC模型由Banker、Charnes和Cooper提出，允许规模报酬可变（Variable Returns to Scale，VRS）。在CCR模型基础上增加约束：

∑_{j=1}^n λ_j = 1

### 2.3 输入导向与输出导向

- 输入导向：在保证输出水平不变的前提下，最小化输入。
- 输出导向：在保证输入水平不变的前提下，最大化输出。

## 3 DEA的效率概念

### 3.1 技术效率

技术效率表示DMU在给定投入下产出的最大化程度。若效率值θ=1且存在可行解，则DMU被视为技术效率。

### 3.2 规模效率

规模效率反映DMU在当前规模下的运行效率。规模效率 = CCR效率 / BCC效率。若规模效率=1，则DMU具有规模效率。

### 3.3 全面效率

全面效率考虑技术效率和规模效率两个方面。CCR模型求得的是全面效率，而BCC模型求得的是纯技术效率。

## 4 DEA的求解流程

1. 收集并整理各DMU的输入输出数据。
2. 选择合适的DEA模型（CCR或BCC，输入导向或输出导向）。
3. 构造线性规划模型并求解效率值。
4. 根据效率值判断DMU是否有效。
5. 分析效差来源，识别冗余投入和不足产出。
6. 进行规模报酬分析和敏感性分析。

## 5 DEA的扩展与应用

### 5.1 超效率DEA

超效率DEA对有效前沿上的DMU进行进一步排序，使效率值可能大于1，适用于对有效DMU进行优先级排序。

### 5.2 网络DEA

网络DEA将DMU拆分为多个相互关联的子过程，分析内部结构和分段效率。

### 5.3 动态DEA

动态DEA考虑多个时期的数据，分析DMU效率随时间的变化。

### 5.4 灰色DEA与模糊DEA

灰色DEA和模糊DEA用于处理不确定性信息和模糊数据。

## 6 DEA的优缺点

### 优点

- 不需要预先设定生产函数形式。
- 能同时处理多输入、多输出问题。
- 可识别有效边界和非效率来源。

### 缺点

- 对数据噪声和异常值敏感。
- 结果是相对效率，仅在所选DMU集合中有效。
- 当指标维度较高时，模型可能产生较多高效率单元，降低区分能力。

## 7 应用场景

DEA广泛应用于：

- 银行、医院、学校等公共部门效率评价
- 企业部门绩效评价
- 能源和环境投入产出分析
- 物流运作效率和供应链绩效分析

## 8 输入导向CCR模型

对待评价单元 $o$，输入导向包络模型为：

$$
\begin{aligned}
\min_{\theta,\lambda}\quad & \theta\\
\text{s.t.}\quad
&X\lambda\leq \theta x_o\\
&Y\lambda\geq y_o\\
&\lambda\geq 0
\end{aligned}
$$

$\theta=1$ 且松弛量为 0 时，该决策单元位于效率前沿。BCC模型再增加 $\mathbf{1}^T\lambda=1$，用于允许可变规模报酬。

```python
import cvxpy as cp
import numpy as np

def dea_ccr_input(X, Y, index):
    X = np.asarray(X, dtype=float)  # 行：投入指标，列：DMU
    Y = np.asarray(Y, dtype=float)  # 行：产出指标，列：DMU
    n = X.shape[1]

    lam = cp.Variable(n, nonneg=True)
    theta = cp.Variable(nonneg=True)
    constraints = [
        X @ lam <= theta * X[:, index],
        Y @ lam >= Y[:, index],
    ]
    problem = cp.Problem(cp.Minimize(theta), constraints)
    problem.solve()
    return theta.value, lam.value
```

DEA评价的是样本内部的相对效率。异常值、指标过多或决策单元太少，都会让大量对象看起来“有效”。投入和产出方向必须有明确业务意义。

## 9 小结

DEA不仅给出效率值，还能通过参考集和松弛量提示改进方向。报告结果时应说明导向、规模报酬假设、指标选择和样本量。

