# 解决 git 没区分大小写而导致文件冲突
git error: The following untracked working tree files would be overwritten by checkout

## 问题回顾
首先，因为与他人协作开发某模块，我（用 macOS）先以 Camel-Case 命名建立文件夹提交，另外一个人(用 Windows)以全小写命名提交。
因为 Windows 区分大小写，而且均未设置 git 的规则为区分大小写导致出现，所以两次提交都视为不同的目录。
然而之前已经处理过一次，就是我把另外一个人的文件移到我创建的 fooBar 目录下，删掉他文件夹再提交。

PS：
但是后来开启 git 大小写敏感后就出现这次的问题
```
$ git checkout feature/foo
error: The following untracked working tree files would be overwritten by checkout:
	src/foobar/index.js
Please move or remove them before you switch branches.
Aborting
```
输入 `$ git ls-files`，出现了两个大小写不同的目录文件
```
src/foobar/index.js
src/fooBar/index.js
```

## 问题解决
1. 首先配置 git 对大小写敏感
```
git config core.ignorecase false
```
2. 执行以下命令
```
git mv src/foobar/index.js src/fooBar/index.js
```
3. 然后提交回仓库就可以了
```
git add .
git commit 
git push 
```
