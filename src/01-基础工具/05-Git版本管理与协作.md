# 第 5 章 · Git 版本管理与协作 

> 这是团队协作最重要的一章。笔记是三个人一起写的，不用 Git 就等于各写各的、最后手动合并——那是灾难。

---

## 5.1 是什么

### Git

**版本管理系统**。通俗说：每次写完东西拍个快照，写坏了可以回退到之前的任何一张快照。它在你电脑上本地运行。

### GitHub

**代码托管平台**。通俗说：把 Git 的快照上传到云端，队友就能看到你的修改、下载你的更新。三个人通过 GitHub 同步。

### 三者关系

```
你的电脑 ←→ GitHub ←→ 队友的电脑
  (Git)              (Git)
```

你改完 → `push` 推到 GitHub → 队友 `pull` 拉到自己电脑。

---

## 5.2 核心概念

| 概念 | 大白话解释 |
|------|-----------|
| **仓库 (repo)** | 一个项目的文件夹，Git 在追踪里面的变化 |
| **提交 (commit)** | 一次"拍快照"，记录了这次改了什么 |
| **推送 (push)** | 把本地的 commit 上传到 GitHub |
| **拉取 (pull)** | 把 GitHub 上的新 commit 下载到本地 |
| **分支 (branch)** | 一条独立的修改路线，不干扰主线路 |
| **合并 (merge)** | 把两条分支的内容合在一起 |
| **冲突 (conflict)** | 两个人同时改同一个文件的同一行，Git 不知道听谁的 |
| **克隆 (clone)** | 把 GitHub 上的仓库整个复制到本地 |
| **暂存区 (stage)** | commit 之前的"购物车"——你先选好哪些文件要放进这次快照 |

---

## 5.3 团队日常工作流

### 场景一：每天开始写笔记前

```bash
# 1. 拉取队友最新更新
git pull origin main
```

### 场景二：写完一节笔记后

```bash
# 2. 看看改了什么
git status

# 3. 把改过的文件加入"购物车"
git add src/02-数学模型/07-优化模型.md

# 4. 提交快照，备注写清楚改了什么
git commit -m "补充 7.1 线性规划的 PuLP 代码示例"

# 5. 推送到 GitHub
git push origin main
```

### 场景三：一条龙命令（最常用）

```bash
git pull origin main           # 拉最新
# ... 写笔记 ...
git add .                      # 把所有修改加入暂存区
git commit -m "写完了 7.2"      # 提交
git push origin main           # 推送
```

---

## 5.4 提交规范

`commit -m` 后面的备注怎么写，约定好：

```
✅ 好的备注：
"补充 7.1 线性规划的代码实现"
"修正第 5 章 Git rebase 描述错误"
"新增附录 C 论文模板"

❌ 坏的备注：
"更新"
"改了一些东西"
"fix"
```

**团队约定**：备注用中文，开头动词统一——`补充`、`修正`、`新增`、`删除`、`重构`。

---

## 5.5 解决冲突

### 冲突是怎么发生的

```
队友改了 07-优化模型.md 第 15 行，推到了 GitHub
你也改了 07-优化模型.md 第 15 行，也推的时候——
Git: "你们俩都改了同一行，我不知道用哪个"
```

### 怎么解决

`git pull` 时会提示冲突文件，打开那个文件，冲突部分长这样：

```markdown
<<<<<<< HEAD
（你写的版本）
=======
（队友写的版本）
>>>>>>> origin/main
```

你需要手动决定**保留哪个**（或整合两者），删掉 `<<<<<<<` `=======` `>>>>>>>` 这些标记，然后：

```bash
git add 解决好的文件.md
git commit -m "解决 07-优化模型.md 冲突"
git push origin main
```

### 如何避免冲突

- **不同人写不同文件**——这是 Markdown 手册体天然的优势
- **开始写之前先 `git pull`**
- **写完了尽快 `push`**，别存一堆再推

---

## 5.6 常用命令速查

| 命令 | 作用 |
|------|------|
| `git status` | 查看当前修改状态 |
| `git diff` | 查看具体改了什么内容 |
| `git log --oneline` | 查看提交历史（压缩成一行的版本） |
| `git pull origin main` | 拉取最新更新 |
| `git add <文件名>` | 把文件加入暂存区 |
| `git add .` | 把当前目录下所有修改加入暂存区 |
| `git commit -m "备注"` | 提交 |
| `git push origin main` | 推送到 GitHub |
| `git clone <仓库地址>` | 克隆仓库到本地 |
| `git checkout -b <分支名>` | 创建并切换到新分支 |
| `git merge <分支名>` | 合并指定分支到当前分支 |
| `git reset --hard HEAD~1` | 撤销最近一次 commit（⚠️ 不可逆） |

---

## 5.7 GitHub 操作指南

### 创建远程仓库

1. 打开 [github.com](https://github.com)，登录
2. 右上角 `+` → `New repository`
3. 填写：
   - Repository name：`math-modeling-notes`（或你们喜欢的名字）
   - Description：数模竞赛团队学习笔记
   - Public / Private：建议 **Private**（比赛期间别公开）
4. 不要勾选 "Add a README file"（本地已经有了）
5. 点 `Create repository`

### 关联本地仓库到 GitHub

GitHub 创建完会显示一段命令，复制运行即可：

```bash
git remote add origin https://github.com/你的用户名/math-modeling-notes.git
git branch -M main
git push -u origin main
```

### 邀请队友协作

GitHub 仓库页面 → `Settings` → `Collaborators` → `Add people` → 输入队友 GitHub 用户名。

队友收到邀请后，在自己的电脑上：

```bash
git clone https://github.com/你的用户名/math-modeling-notes.git
cd math-modeling-notes
```

之后就是上面 5.3 节的日常流程。

---

## 5.8 `.gitignore` 配置

有些文件不想上传 GitHub（临时文件、PDF 导出产物、系统文件），在仓库根目录创建 `.gitignore`：

```gitignore
# 系统文件
.DS_Store
Thumbs.db

# PDF 导出产物
*.pdf

# Python 虚拟环境
venv/
.venv/

# VS Code 个人配置
.vscode/settings.json

# 笔记草稿
*.draft.md
```

> PDF 不加 `.gitignore` 也可以——把最终导出的 PDF 也推上去，队友不用自己生成就能看。你们自己决定。

---

## 5.9 团队协作最佳实践

1. **每人认领章节写**——你写第二卷、队友写第一卷，天然不冲突
2. **写完一节推一次**——不要攒一周再推
3. **push 前先 pull**——养成肌肉记忆
4. **重要修改先在群里说一声**——"我要动 07 章了，你们别改"
5. **不要上传大文件**（>10MB 的数据集、论文 PDF）——用 `.gitignore` 排除，或放网盘分享链接
6. **commit 备注写清楚**——三个月后你自己回来看，忘了 `"更新"` 到底是更新了什么

---

> 📖 **扩展阅读**：
> - Git 官方教程: https://git-scm.com/book/zh/v2
> - GitHub 快速入门: https://docs.github.com/zh/get-started
> - 可视化学习 Git 分支: https://learngitbranching.js.org/
