# 原始仓库更新了 合并到我 fork 的仓库

要将原始仓库（`upstream`）的 **主分支（如 `master` 或 `main`）** 的更新合并到你自己的 Fork 仓库中，可以按照以下步骤操作：

---

### ✅ 前提条件

确保你已经配置了远程仓库：

```bash
git remote -v
```

你应该看到如下内容：

```
origin   git@github.com:Wheat-s/qwerty-learner.git (fetch)
origin   git@github.com:Wheat-s/qwerty-learner.git (push)
upstream git@github.com:RealKai42/qwerty-learner.git (fetch)
upstream git@github.com:RealKai42/qwerty-learner.git (push)
```

如果没有，请先添加：

```bash
git remote add upstream git@github.com:RealKai42/qwerty-learner.git
```

---

### 🔄 步骤：同步上游主分支到你的 Fork

#### 1. 确保你在本地 `master` 分支上

```bash
git checkout master
```

#### 2. 拉取上游仓库最新内容

```bash
git fetch upstream
```

这会下载原始仓库的所有分支和提交，但不会自动合并。

#### 3. 合并上游主分支到本地主分支

```bash
git merge upstream/master --ff-only
```

> 使用 `--ff-only` 表示只允许快进合并（fast-forward），避免自动产生合并提交。如果存在冲突或无法快进，Git 会提示你处理。

如果你看到类似错误：

```
fatal: Not possible to fast-forward, aborting.
```

说明你的本地 `master` 分支有额外的提交（比如你做了修改），这时你可以使用普通 merge 或 rebase（见下文可选步骤）。

#### 4. 推送更新到你的 Fork

```bash
git push origin master
```

这样你就成功将原始仓库主分支的更新同步到了你的 Fork 中。

---

### 🧠 可选：使用 rebase 避免合并提交（推荐）

如果你希望保持提交历史线性、干净，可以使用 `rebase` 替代 `merge`：

```bash
git fetch upstream
git rebase upstream/master
```

解决可能的冲突后继续 rebase：

```bash
git add .
git rebase --continue
```

最后强制推送更新到你的 Fork（注意：需要加 `--force-with-lease`）：

```bash
git push origin master --force-with-lease
```

⚠️ 注意：`--force-with-lease` 是安全的强制推送方式，避免误覆盖他人提交。

---

### 🔒 注意事项

- 本流程只会同步 `master` 分支，**不会影响其他分支**
- 如果你之前创建了自己的功能分支（如 `feat/local-changes`），不要在这个分支上执行这些操作
- 推荐在合并前备份重要更改，尤其是多人协作的分支

---

### ✅ 总结：完整命令速查

```bash
# 切换到主分支
git checkout master

# 获取上游更新
git fetch upstream

# 快进合并（推荐）
git merge upstream/master --ff-only

# 或使用 rebase（更干净）
git rebase upstream/master

# 推送到你的 Fork
git push origin master
# 或使用 force push（使用 rebase 后）
git push origin master --force-with-lease
```
