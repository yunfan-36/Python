#### git使用命令指南

```python
git add . # 添加文件夹下所以文件

git commit -m "版本1"

git log # 查看记录

git reflog # 显示之前的命令

git reset --hard HEAD^ # 回到第一个版本

git reset --hard 版本号哈希值 # 回到某个版本

HEAD^ # 第一个版本

HEAD^^ # 第二个版本

HEAD~1 # 第一个版本

HEAD~2 # 第二个版本

git checkout -- wenjian.txt #丢弃修改

git add wenjian.txt # 添加到暂存区

git reset HEAD wenjian.txt # 取消添加到暂存区

git status -uno # 只显示仓库文件

git log --pretty=oneline # 简单查看log

git log --pretty=oneline --graph # 简单查看log，并且有图形
```

```python
git 分支概念

git branch # 查看分支

git brach <name> # 创建分支

git checkout <name> # 切换分支

git checkout -b hzb # 创建一个hzb分支并且切换至hzb分支

git merge <name> # 合并某分支到当前分支

git branch -d <name> # 删除分支
git branch -a # 查看所有分支，包括远程分支
git branch # 查看本地分支
git push origin --delete hzb  # 删除远程分支hzb
```

```python
bug分支
git stash # 保存当前工作现场
切换到bug所在分支，并创建切换到一个临时分支，修复bug
git checkout -b bug分支
修复完bug之后，切换回bug所在分支并合并临时分支，合并使用no-ff模式 git merge --no-ff -m "修复bug" 临时分支名
切换回工作分支
删除临时分支
git stash pop # 恢复工作现场
```

```python
github

ssh-keygen -t rsa -C "注册邮箱" 生成ssh密钥
git clone 加链接 # 克隆
git push origin hzb # 添加远程分支hzb
git push origin --delete hzb  # 删除远程分支hzb
git branch --set-upstream-to=origin/远程分支名 本地分支名 # 将本地分支跟踪服务器分支
git pull origin master # 拉取远程分支
```

#### 个人总结常用git命令

```python
# 最常用的个命令
git add <name> # 添加到暂存区
git commit -m '版本号' # 本地版本管理
git push # 推送到github
git pull origin <name> # 从github拉取某个分支
git clone <github-link> # 从github克隆
```



