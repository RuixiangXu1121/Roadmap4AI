# Git 操作指南

## 一、初始化并推送到 GitHub（完整流程）

### 1. 初始化 Git 仓库
```bash
git init
```

### 2. 创建 .gitignore
在项目根目录创建 `.gitignore` 文件：
```
# Python
__pycache__/
*.pyc
venv/
.venv/
*.egg-info/

# 环境变量
.env
.env.local

# 大文件目录（可选）
Books/
leedl-tutorial/
```

**注意事项**：
- **创建时机**：先创建 `.gitignore`，再 `git add`
- **格式**：不要加 `/` 前缀（`Books/` 而不是 `/Books`）
- **立即生效**：已跟踪的文件需要 `git rm --cached` 才能忽略

### 3. 添加文件并提交
```bash
git add -A
git commit -m "Initial commit"
```

### 4. 关联 GitHub 仓库（两种方式）

**方式一：已有仓库**
```bash
git remote add origin https://github.com/用户名/仓库名.git
```

**方式二：创建新仓库**
```bash
gh repo create 仓库名 --public
```

### 5. 推送代码
```bash
git branch -M main              # 将分支改名为 main
git push -u origin main         # 推送到远程
```

---

## 二、常用命令速查

### 查看状态
```bash
git status                      # 查看工作区状态
git log --oneline               # 查看提交历史
```

### 文件操作
```bash
git add 文件名                  # 添加单个文件
git add -A                      # 添加所有文件（包括删除）
git add .                       # 添加当前目录（不包括删除）
git rm --cached 文件            # 取消跟踪文件（保留本地）
git rm 文件                      # 删除文件（同时删除本地）
```

### 删除文件（两种方式）
| 场景 | 命令 |
|------|------|
| 本地远程都删除 | 直接删除文件夹 + `git add -A` + commit + push |
| 保留本地，删除远程 | `git rm --cached 文件/目录` + commit + push |

### 提交操作
```bash
git commit -m "提交说明"        # 提交
git commit --amend              # 修改最后一次提交
```

### 分支操作
```bash
git branch                      # 查看分支
git branch -M main              # 重命名分支
git checkout -b 分支名          # 创建并切换分支
```

### 远程操作
```bash
git remote -v                  # 查看远程仓库
git push                       # 推送到远程
git push -u origin main         # 首次推送并设置上游
git push --force                # 强制推送
git pull                       # 拉取并合并
```

---

## 三、大文件处理（Git LFS）

### 1. 安装 Git LFS
```bash
# Ubuntu/Debian
curl -sL https://github.com/git-lfs/git-lfs/releases/download/v3.5.1/git-lfs-linux-amd64-v3.5.1.tar.gz | tar xz && cd git-lfs-* && sudo install git-lfs /usr/local/bin/

# macOS
brew install git-lfs
```

### 2. 初始化 LFS
```bash
git lfs install
```

### 3. 跟踪大文件
```bash
git lfs track "*.pdf"           # 跟踪所有 PDF
git lfs track "目录/*"           # 跟踪目录下的文件
```

### 4. 查看跟踪的文件
```bash
git lfs ls-files
```

---

## 四、常见问题

### 1. 首次推送前检查
```bash
git status          # 查看哪些文件会被跟踪
git ls-files        # 查看已被跟踪的文件
```

### 2. GitHub 文件大小限制
| 大小 | 后果 |
|------|------|
| > 50MB | 警告 |
| > 100MB | 拒绝 |

**解决**：使用 Git LFS 或不上传

### 3. 常见错误速查
| 错误 | 原因 | 解决 |
|------|------|------|
| 推送到错误仓库 | 没检查远程地址 | `git remote -v` |
| 文件删了还在 GitHub | 需 `git add -A` | 删除后执行 add |
| .gitignore 不生效 | 文件已被跟踪 | `git rm --cached` |
| push 被拒绝 | 远程有更新 | `git pull` 后再 push |

### 4. 放弃本地修改
```bash
git checkout -- 文件            # 放弃单个文件修改
git reset --hard HEAD           # 放弃所有修改
```

### 5. 删除远程文件（保留本地）
```bash
git rm --cached 文件            # 取消跟踪
git commit -m "Remove file"    # 提交
git push                        # 推送
```

### 6. 从 Git 历史中删除文件
```bash
git filter-branch --tree-filter 'rm -f 文件路径' HEAD
git push --force
```

### 7. 撤销提交
```bash
git reset --soft HEAD~1         # 撤销提交，保留修改
git reset --hard HEAD~1         # 撤销提交，丢弃修改
```

---

## 五、最佳实践

### 新项目初始化流程
```bash
# 1. 创建项目目录
mkdir myproject
cd myproject

# 2. 初始化 Git
git init

# 3. 创建 .gitignore（第一时间创建）
echo "venv/
__pycache__/
.env" > .gitignore

# 4. 检查状态
git status

# 5. 添加文件并提交
git add -A
git commit -m "Initial commit"

# 6. 关联远程仓库
gh repo create myproject --public

# 7. 推送
git push -u origin main
```

---

## 六、工作流程示意

```
工作区 → 暂存区 → 本地仓库 → 远程仓库
  ↓        ↓         ↓         ↓
git add  git commit  git push
```

1. **修改文件** → 工作区
2. **git add** → 暂存区
3. **git commit** → 本地仓库
4. **git push** → 远程仓库