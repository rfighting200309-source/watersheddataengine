# 🚀 Claude Code + Git/GitHub 协作教学

> 写给想用 Git 控制版本、回退代码，但不知道怎么上手的你。  
> 只讲最常用、最大众的操作，30 分钟学会。

---

## 一、先理解三个角色

| 角色 | 是什么 | 类比 |
|------|--------|------|
| **工作区** | 你电脑上的文件夹（`D:\gitgithub`） | 你的书桌 |
| **本地仓库** | 隐藏文件夹 `.git/`，记录每次改动 | 你的日记本 |
| **GitHub（远程）** | 网上的备份，可以多人协作 | 云盘 |

```
你的电脑（工作区 + 本地仓库）  ──push──▶  GitHub（远程仓库）
                                ◀──pull──
```

---

## 二、你目前的环境

你的项目已经配置好了：

```
本地路径:  D:\gitgithub
远程地址:  git@github.com:rfighting200309-source/watersheddataengine.git
当前分支:  main
```

> ✅ 后面的命令都在 `D:\gitgithub` 目录下执行。

---

## 三、最核心的 4 个操作（每天都会用）

### 1️⃣ 查看当前状态

```bash
git status
```

这个命令告诉你：**改了哪些文件、哪些还没保存**。  
每次动手前先看一眼，心里有数。

```
典型输出：
  modified:   README.md          ← 改过的文件
  Untracked files:  newfile.py  ← 新建的文件，Git 还没管它
```

### 2️⃣ 保存改动（提交）

```bash
# 第一步：把文件加入"待保存清单"
git add .

# 第二步：正式保存，附上一句话说明改了什么
git commit -m "修复了登录页面的bug"
```

> 💡 `git add .` 的 `.` 代表当前目录下所有改动。最常用。

### 3️⃣ 上传到 GitHub（备份）

```bash
git push
```

第一次推送用：`git push -u origin main`  
之后只需要：`git push`

### 4️⃣ 从 GitHub 下载最新代码

```bash
git pull
```

---

## 四、日常工作流程（记住这个就够了）

```
┌─────────────────────────────────────────────────────┐
│                                                      │
│   ① git pull        拉取最新代码（开始工作前）        │
│         │                                            │
│         ▼                                            │
│   ② 写代码...       Claude Code 帮你写 / 自己写      │
│         │                                            │
│         ▼                                            │
│   ③ git status      看看改了什么                     │
│         │                                            │
│         ▼                                            │
│   ④ git add .       加入所有改动                      │
│         │                                            │
│         ▼                                            │
│   ⑤ git commit -m "做了什么"    保存                 │
│         │                                            │
│         ▼                                            │
│   ⑥ git push        推到 GitHub（备份）               │
│                                                      │
└─────────────────────────────────────────────────────┘
```

### 实操例子：

```bash
# 早上开始工作
cd /d/gitgithub
git pull

# ... 用 Claude Code 写了很多代码 ...

# 下班前保存
git status              # 看看改了什么
git add .               # 全部加入
git commit -m "完成了用户注册功能"   # 保存
git push                # 推到 GitHub 备份
```

---

## 五、回退代码（救命操作）

这是 Git 最有用的功能。按危险程度从低到高排列：

### 🟢 场景 1：改坏了，想回到上次保存的状态

```bash
# 丢弃某个文件的所有改动（回到上次 commit 的状态）
git checkout -- 文件名

# 例如：
git checkout -- README.md
```

### 🟢 场景 2：git add 了但还没 commit，想撤销

```bash
# 把某个文件从"待保存清单"中移除（文件改动还在）
git reset HEAD 文件名

# 把所有文件移出待保存清单
git reset HEAD .
```

### 🟡 场景 3：已经 commit 了但还没 push，想撤销那次 commit

```bash
# 撤销最近一次 commit，但改动保留在工作区（最安全）
git reset --soft HEAD~1

# 撤销最近一次 commit，改动也丢弃（⚠️ 谨慎）
git reset --hard HEAD~1
```

| 参数 | commit | 改动 |
|------|--------|------|
| `--soft` | 撤销 ✅ | 保留 ✅ |
| `--mixed`（默认） | 撤销 ✅ | 保留但回到"未 add"状态 |
| `--hard` | 撤销 ✅ | 丢弃 ❌ |

> 💡 新手用 `--soft` 最安全。

### 🔴 场景 4：已经 push 到 GitHub 了，想回退

```bash
# 回退本地到上一个版本
git reset --soft HEAD~1

# 强制推送到 GitHub（覆盖远程）
git push --force
```

> ⚠️ `--force` 很危险：如果你是唯一用这个仓库的人，没问题；如果有别人也在用，不要 force push。

### 📋 场景 5：查看所有历史版本

```bash
# 查看提交历史（简洁版）
git log --oneline

# 输出示例：
#   8bfd3d8 Initial commit
#   a1b2c3d 添加了登录功能
#   d4e5f6g 修复了首页样式
```

每一个 `8bfd3d8` 这样的字符串就是一个**版本号**（commit hash），你可以回到任意一个版本：

```bash
# 回到任意历史版本看看代码（只读模式）
git checkout 8bfd3d8

# 回到最新版本
git checkout main
```

---

## 六、分支（进阶但推荐）

分支让你可以"另开一条路"做实验，不影响主干。

```bash
# 创建并切换到新分支
git checkout -b 实验新功能

# 在分支上随便改、随便 commit，不影响 main

# 切回主分支
git checkout main

# 把实验分支合并到主分支
git merge 实验新功能

# 删除不再需要的分支
git branch -d 实验新功能
```

> 在 Claude Code 中，你可以让 AI 在一个分支上工作，验证没问题后再合并到 main。

---

## 七、和 Claude Code 配合的最佳实践

### ✅ 每次让 Claude Code 完成一个功能后：

```bash
git add .
git commit -m "Claude Code 完成了 XXX 功能"
```

### ✅ commit 信息的写法规范：

```bash
# 好的 commit 信息（动词开头，说清楚做了什么）
git commit -m "添加用户登录接口"
git commit -m "修复导航栏在手机上的显示问题"
git commit -m "重构数据库连接模块"

# 不好的 commit 信息
git commit -m "修改"        ← 太模糊
git commit -m "123"         ← 不知道干了啥
```

### ✅ 每个 commit 只做一件事：

```
❌ 错误：一个 commit 既改了登录、又改了样式、还加了新页面
✅ 正确：三个 commit，各管各的
```

这样回退时可以精确回退，不会"一刀切"。

---

## 八、常见问题速查

| 问题 | 解决 |
|------|------|
| `git push` 失败，提示远程有新代码 | 先 `git pull`，再 `git push` |
| pull 时有冲突（conflict） | 打开冲突文件，手动合并 `<<<<<<<` 和 `>>>>>>>` 标记，然后 `git add .` + `git commit` |
| 不小心 `git add` 了不该加的文件 | `git reset HEAD 文件名` |
| 想临时保存工作，切换去干别的事 | `git stash`（暂存），回来 `git stash pop` |
| 想看某个文件在某个版本时长什么样 | `git show 版本号:文件路径` |

---

## 九、你的项目当前状态

```
📁 D:\gitgithub
  ├── README.md          ← 已提交
  ├── .gitignore         ← 已提交（告诉 Git 忽略哪些文件）
  └── Git与ClaudeCode协作教学.md  ← 本教程（等你提交）
```

你现在就可以试试：

```bash
cd /d/gitgithub
git add .
git commit -m "添加 Git 协作教学文档"
git push
```

---

## 十、一句话总结

> **改完代码 → `git add .` → `git commit -m "消息"` → `git push`**  
> 这就够了。回退用 `git log --oneline` 找到版本号，然后 `git reset --soft 版本号`。

其他高级功能等你需要时再学，先把这四个命令用熟。
