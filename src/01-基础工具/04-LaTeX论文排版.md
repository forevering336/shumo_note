# 第 4 章 · LaTeX 论文排版 🟣 ⭐

> 数模论文的排版标准答案就是 LaTeX。Word 也能写，但公式一多、引用一多，LaTeX 甩 Word 几条街。

---

## 4.1 LaTeX 是什么

一种**标记语言**：你写内容和结构，它帮你排成印刷级的 PDF。跟 Markdown 思路类似但功能更强——公式、交叉引用、参考文献、图表浮动排版，全是自动的。

### LaTeX vs Markdown

| | Markdown | LaTeX |
|------|----------|-------|
| 上手难度 | 10 分钟 | 半天 |
| 公式 | 支持（MathJax） | 原生支持，更强大 |
| 自动编号 | 无 | 图、表、公式、参考文献全自动 |
| 适合写 | 笔记、博客 | 论文、报告 |
| 可读性 | 源码易读 | 源码稍乱 |

**分工**：笔记用 Markdown → 导出 PDF 看；比赛论文用 LaTeX → 直接交。

---

## 4.2 环境安装

**最省事的方案**：用 Overleaf（在线 LaTeX 编辑器，浏览器打开就能用）

- 网址：https://www.overleaf.com
- 注册 → 新建项目 → 选模板 → 开始写
- 优点：不用装任何东西，云端编译，队友可以协作
- 缺点：需要联网

**本地方案**（离线也能用）：

```bash
# Windows: 装 MiKTeX https://miktex.org/download
# 或者 TeX Live https://tug.org/texlive/
# VS Code 装 LaTeX Workshop 插件
```

**推荐比赛时用 Overleaf + 本地双保险**。Overleaf 写完，下载 .zip 备份到本地。

---

## 4.3 一个最简示例

```latex
\documentclass[12pt,a4paper]{article}
\usepackage[UTF8]{ctex}        % 中文支持
\usepackage{amsmath,amssymb}   % 数学符号
\usepackage{graphicx}          % 插图
\usepackage{geometry}
\geometry{margin=2.5cm}

\title{2026 年全国大学生数学建模竞赛\\B 题论文}
\author{队伍编号：123456}
\date{2026 年 9 月}

\begin{document}
\maketitle

\begin{abstract}
本文针对 xxx 问题，建立了 xxx 模型...
\end{abstract}

\section{问题重述}
...

\section{模型假设与符号说明}
...

\section{模型的建立与求解}
...

\subsection{线性规划模型}

目标函数：
\begin{equation}
    \min Z = \sum_{i=1}^{n} c_i x_i
    \label{eq:obj}
\end{equation}

约束条件：
\begin{equation}
    \sum_{i=1}^{n} a_{ji} x_i \leq b_j, \quad j = 1,2,\ldots,m
    \label{eq:constraint}
\end{equation}

如公式 \eqref{eq:obj} 和公式 \eqref{eq:constraint} 所示...

\section{模型评价与改进}
...

\begin{thebibliography}{9}
\bibitem{ref1} 某某某. 数学建模方法与应用. 高等教育出版社, 2020.
\end{thebibliography}

\end{document}
```

---

## 4.4 数模常用 LaTeX 语法速查

### 公式

```latex
% 行内公式
$f(x) = \sum_{i=1}^{n} x_i^2$

% 独立公式（无编号）
\[
    \frac{\partial u}{\partial t} = \alpha \nabla^2 u
\]

% 独立公式（有编号，可引用）
\begin{equation}
    P(A|B) = \frac{P(B|A)P(A)}{P(B)}
    \label{eq:bayes}
\end{equation}

% 多行对齐
\begin{align}
    y &= x_1 + 2x_2 + 3x_3 \\
      &\leq 100 \nonumber
\end{align}

% 矩阵
\begin{bmatrix}
    a_{11} & a_{12} \\
    a_{21} & a_{22}
\end{bmatrix}

% 分段函数
f(x) = \begin{cases}
    0, & x < 0 \\
    x^2, & 0 \leq x \leq 1 \\
    1, & x > 1
\end{cases}
```

### 图片

```latex
\begin{figure}[htbp]          % h=here, t=top, b=bottom, p=page
    \centering
    \includegraphics[width=0.8\textwidth]{figure1.png}
    \caption{模型流程图}
    \label{fig:flow}
\end{figure}
```

### 表格

```latex
\begin{table}[htbp]
    \centering
    \caption{不同模型结果对比}
    \label{tab:comparison}
    \begin{tabular}{|c|c|c|c|}
        \hline
        模型 & 准确率 & 运行时间(s) & 内存(MB) \\
        \hline
        线性规划 & 92.3\% & 0.5 & 128 \\
        随机森林 & 95.1\% & 12.3 & 1024 \\
        \hline
    \end{tabular}
\end{table}
```

### 列表

```latex
\begin{itemize}
    \item 第一条
    \item 第二条
\end{itemize}

\begin{enumerate}
    \item 第一步
    \item 第二步
\end{enumerate}
```

---

## 4.5 数模论文模板推荐

在 Overleaf 或本地直接搜这些关键词：

- 中文模板：`CUMCM` `数学建模国赛模板`
- 英文模板：`MCM thesis template`

网上有很多开箱即用的——换标题、换内容就能交。

---

## 4.6 常见坑

- ⚠️ 中文论文一定要加 `\usepackage[UTF8]{ctex}`，不然中文不显示
- ⚠️ 图片路径用相对路径，图片和 `.tex` 放同一文件夹
- ⚠️ Overleaf 免费版编译时间有限（约 1 分钟），大论文可能超时 → 本地编译
- ⚠️ 提交前一定检查所有 `\ref{}` 是否正确引用，Overleaf 爆红的地方就是有问题的
- ⚠️ 不要手动编号（图 1、表 2、公式 3）——让 LaTeX 自动编号，改顺序也不用改数字

---

> 📖 **参考**：
> - Overleaf 中文教程: https://cn.overleaf.com/learn/latex/Chinese
> - LaTeX 数学公式速查: https://en.wikibooks.org/wiki/LaTeX/Mathematics
