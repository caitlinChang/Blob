git 配置级别
local > global > system

```
git config --global --list

// 清空global账号信息
git config --global --unset user.name
git config --global --unset user.email
```

### git subtree & git submodule

#### 区别

1. `git submodule` 需要在主项目根目录下添加`.gitmodule`文件来记录引入的子项目，git subtree 完全透明

2. `git subtree` 相当于 clone 了子项目，可以跟着主项目的迭代进度进行 commit、push，`git submodule` 相当于子项目的引用，任何子项目的改动需要同步
3.

#### git subtree 常用命令

```
git subtree pull --prefix=<本地项目名> <项目git地址> <分支> <ref>
```
