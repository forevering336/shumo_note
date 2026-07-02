# 附录 A · 常用公式卡

> 供论文写作时快速查阅，按模型类型分类。

---

## 优化模型相关

### 线性规划标准形式

$$
\begin{aligned}
\min \quad & \mathbf{c}^T \mathbf{x} \\
\text{s.t.} \quad & A\mathbf{x} \leq \mathbf{b} \\
& \mathbf{x} \geq 0
\end{aligned}
$$

### 拉格朗日函数

$$
\mathcal{L}(\mathbf{x}, \lambda) = f(\mathbf{x}) + \sum_{i=1}^{m} \lambda_i g_i(\mathbf{x})
$$

### KKT 条件

$$
\begin{aligned}
\nabla f(\mathbf{x}^*) + \sum \lambda_i^* \nabla g_i(\mathbf{x}^*) &= 0 \\
\lambda_i^* g_i(\mathbf{x}^*) &= 0 \quad (\text{互补松弛}) \\
\lambda_i^* &\geq 0
\end{aligned}
$$

---

## 微分方程相关

### 一阶线性 ODE

$$
\frac{dy}{dx} + P(x)y = Q(x)
$$

通解：

$$
y = e^{-\int P dx} \left[ \int Q e^{\int P dx} dx + C \right]
$$

### Logistic 方程（种群模型）

$$
\frac{dN}{dt} = rN\left(1 - \frac{N}{K}\right)
$$

解析解：

$$
N(t) = \frac{K}{1 + \left(\frac{K}{N_0} - 1\right)e^{-rt}}
$$

---

## 概率统计相关

### 贝叶斯公式

$$
P(A|B) = \frac{P(B|A) \cdot P(A)}{P(B)}
$$

### 正态分布

$$
f(x) = \frac{1}{\sigma\sqrt{2\pi}} \exp\left(-\frac{(x-\mu)^2}{2\sigma^2}\right)
$$

### 最小二乘估计

$$
\hat{\beta} = (X^T X)^{-1} X^T \mathbf{y}
$$

---

## 评价决策相关

### TOPSIS 相对贴近度

$$
C_i = \frac{D_i^-}{D_i^+ + D_i^-}
$$

其中 $D_i^+$ 为到正理想解的距离，$D_i^-$ 为到负理想解的距离。

### 熵权法权重

$$
w_j = \frac{1 - e_j}{\sum_{k=1}^{n}(1 - e_k)}
$$

其中 $e_j = -\frac{1}{\ln m} \sum_{i=1}^{m} p_{ij} \ln p_{ij}$ 为第 $j$ 个指标的信息熵。

---

## 图论相关

### Dijkstra 算法复杂度

$$
O(|E| + |V| \log |V|)
$$

### 最大流最小割定理

$$
\text{最大流} = \text{最小割容量}
$$

---

> 💡 持续补充中。每学一个新模型，把最核心的公式添加到这里。
