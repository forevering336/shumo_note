# 附录 D · 资源索引

资源索引优先保留官方文档和竞赛组委会入口。博客和公开视频适合帮助理解，但涉及函数参数、竞赛规则和论文格式时，应回到官方来源确认。

> 链接核对日期：2026-07-29

---

## 1 竞赛信息

- [全国大学生数学建模竞赛官网](https://www.mcm.edu.cn/)：通知、赛题、参赛规则和获奖信息。
- [2026 年全国大学生数学建模竞赛论文格式规范](https://www.mcm.edu.cn/upload_cn/node/775/cQMeL0YY905244c8bd4b9af832f1699446d8385e.pdf)：提交前逐条核对，不沿用旧年份模板。
- [中国大学生在线数学建模专题](https://dxs.moe.gov.cn/zx/hd/sxjm/)：竞赛公告及相关通知的发布入口。

竞赛时间、页数、文件大小和支撑材料要求可能调整，每年赛前重新下载规则。

---

## 2 Python 与科学计算

- [Python 官方教程](https://docs.python.org/3/tutorial/)：语言基础、模块、文件与异常。
- [NumPy 文档](https://numpy.org/doc/stable/)：数组、线性代数和随机数。
- [pandas 文档](https://pandas.pydata.org/docs/)：表格数据读写、清洗、分组和时间序列。
- [SciPy 文档](https://docs.scipy.org/doc/scipy/)：优化、积分、统计、插值和信号处理。
- [Matplotlib 文档](https://matplotlib.org/stable/)：绘图接口与示例。

查询函数时先确认本机版本，再选择对应版本的文档。旧代码能运行不代表参数仍是推荐写法。

---

## 3 统计与机器学习

- [statsmodels 文档](https://www.statsmodels.org/stable/)：回归、统计检验和时间序列。
- [scikit-learn 用户指南](https://scikit-learn.org/stable/user_guide.html)：预处理、模型、交叉验证和评价指标。
- [XGBoost 文档](https://xgboost.readthedocs.io/en/stable/)：梯度提升树及 sklearn 接口。
- [PyTorch 文档](https://pytorch.org/docs/stable/)：张量、自动微分和神经网络。

机器学习示例中的数据划分方式比模型参数更值得优先检查。任何预处理都应放在训练折内部完成。

---

## 4 优化与图模型

- [CVXPY 文档](https://www.cvxpy.org/)：凸优化建模及求解器接口。
- [OR-Tools 文档](https://developers.google.com/optimization)：整数规划、路径、指派和约束规划。
- [NetworkX 文档](https://networkx.org/documentation/stable/)：图结构、最短路、网络流和图算法。
- [SciPy Optimization](https://docs.scipy.org/doc/scipy/reference/optimize.html)：连续优化、线性规划和最小二乘。

使用求解器时记录求解状态、目标值、约束违反量和运行时间。只保存决策变量不利于排查无界、不可行和提前停止。

---

## 5 排版与协作

- [LaTeX Project](https://www.latex-project.org/help/documentation/)：LaTeX 官方文档入口。
- [Git 官方文档](https://git-scm.com/doc)：版本管理命令和教程。
- [GitHub 文档](https://docs.github.com/)：远程仓库、分支、Pull Request 和协作。
- [Pandoc 用户指南](https://pandoc.org/MANUAL.html)：Markdown 与 PDF、Word 等格式转换。

团队使用同一份模板时，字体、编译方式和依赖版本也应进入仓库说明，避免只在某一台电脑上能导出。

---

## 6 数据来源记录

下载外部数据时同步记录：

```text
数据名称：
发布机构：
原始链接：
下载日期：
时间范围：
字段与单位：
许可证或使用限制：
本地文件名：
清洗脚本：
```

网页可能更新或失效。对论文结论关键的数据，建议保留原始文件、元数据和下载日期，并在支撑材料中说明来源。
