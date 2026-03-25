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
git add -A                      # 添加所有文件
git add .                       # 添加当前目录（不包括删除）
git rm --cached 文件            # 取消跟踪文件
```

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

### 1. 放弃本地修改
```bash
git checkout -- 文件            # 放弃单个文件修改
git reset --hard HEAD           # 放弃所有修改
```

### 2. 删除远程文件（保留本地）
```bash
git rm --cached 文件            # 取消跟踪
git commit -m "Remove file"    # 提交
git push                        # 推送
```

### 3. 从 Git 历史中删除文件
```bash
git filter-branch --tree-filter 'rm -f 文件路径' HEAD
git push --force
```

### 4. 撤销提交
```bash
git reset --soft HEAD~1         # 撤销提交，保留修改
git reset --hard HEAD~1         # 撤销提交，丢弃修改
```

---

## 五、工作流程示意

```
工作区 → 暂存区 → 本地仓库 → 远程仓库
  ↓        ↓         ↓         ↓
git add  git commit  git push
```

1. **修改文件** → 工作区
2. **git add** → 暂存区
3. **git commit** → 本地仓库
4. **git push** → 远程仓库