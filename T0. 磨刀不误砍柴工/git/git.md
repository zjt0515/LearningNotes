```
git init
git add .
git commit -m ""
git remote add origin [url]
git push -u origin main
```

## 初始化

`git init`

`git add .`添加到暂存区

`git commit -m ["note"]`

`git branch -M main`修改分支名

`git remote add origin [url]`

`git pull -u origin [远程分支](:[本地分支])`

`git pull origin [分支]`提交



## 信息查询

`git config --list`查询配置

`git branch`查看当前分支

`git status`仓库状态

`git log`提交记录



## 分支管理

`git branch -vv` 查看分支追踪关系

`git branch`本地分支

`git branch -r`远程分支

`git branch [分支名]`

`git checkout (-b) [分支名]`切换分支

`git merge [被合并的分支]`合并分支