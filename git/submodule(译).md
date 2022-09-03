> 本篇文章译自[Git Submodules basic explanation](https://gist.github.com/gitaarik/8735255)

# Git Submodules basic explanation

### submodule 的作用

在 Git 中，你可以为一个仓库添加 submodule，本质上是在主仓库中再嵌入一个仓库，这种操作可以很有用，它有一下几点优势：

- 可以分离代码到不同的独立仓库中
  如果你有一个很大的代码库，有很多模块，你可以将每一个模块都拆成 submodule，这样你就可以拥有一个清晰的 git log，每一个 commit 都可以明确为一个模块的改动；

- 可以将一个 submodule 添加到多个仓库中
  如果你有多个项目分享一个模块的需要，submodule 可以派上用场；使用 submodule 你可以很容易在任何项目中更新以 submodule 形式接入的模块，这比直接复制粘贴模块代码到项目里会更容易；

### 基础

当你在一个 git 项目中添加 submodule 时，你不是添加 submodule 的代码，你只是添加了 submodule 的信息到主项目中；这些信息描述了当前的 submodule 指向的 commit；这种方式可以保证 submodule 不会自动更新远端仓库的代码，同时当项目的协同开发者拉取 submodule 代码时，可以保证他们拉下来的也是一样的；

### 添加 submodule

你可以通过下述命令在主项目中添加 submodule ：

```
git submodule add url
```

默认配置下，这行命令会检查 submodule 的 git 仓库的到 主项目中的 submodule 目录下，并且会添加信息到主项目，这信息包含了 commit 的指向，

在这个操作过后，如果你执行`git status`，你会看到两个文件在修改区，一个是`.gitmodules`文件，另一个是 sumodule 的路径；当你 commit 并且 push 这些文件时，你就把 submodule commit/push 到了远端；

### 获取 submodule 的代码

如果一个人创建一个新的 submodule, 同一个 team 的其他人需要；首先你需要获取这个 submodule 的信息，这个信息是可以通过`git pull`拉取下来的。如果有新的 submodule,你也会用过 `git pull`一并获取；然后你需要通过下面这行命令初始化 submodule

```
git submodule init
```

如果你 clone 了一个使用 submodule 的仓库，你需要执行这行命令去获取 submodule 的 code；当然呢你也可以通过`git clone --recurse-submodules`一并拉下拉主项目和 submodules 的代码；

### 在 submodule 中推送更新

submodule 是一个独立的仓库，如果你想要在主项目的 submodule 中修改代码，你需要进入 submodule 的目录，然后再执行 commit/push 这些操作；但是你需要让主项目知道你修改 submodule 的代码，并且更新主项目中记录的 submodule 的 commit 指向信息；

如果你修改了 submodule 的代码，那你在主项目下执行`git status`，将会看到`当前commit中的修改未暂存`，并且在这行文本之前会展示修改的内容；这意味着 submodule 的代码被 checkout 到一个新的 commit，这个 commit 并非是是当前主项目的.gitmodules 信息中记录的 commit；为了使主项目指向这个被创建的新的 commit, 你需要在主项目中创建一个新的 commit；

下一步是描述不同的场景

#### 在 submodule 中作出变更

- `cd` 进入 submodule 所在的目录
- 修改点代码
- `git commit` 你做出的修改
- `git push` 这个 commit
- `cd` 进去主项目
- `git status`
- `git diff`
- `git commit`，它会更新 submodule commit；

#### 更新 submodule 指向不同的 commit

- `cd` 进入 submodule 所在的目录
- `git checkout` 切换到任意一个分支/commit
- `cd` 回到主项目目录
- `git status`你会看到 submodule 路径下的变更
- `git diff`
- `git commit`

#### 如果其他人更新了 submodule pointer

如果某个人更新了 submodule，其他的 team-members 应该更新 submodules 的代码；这并不是在你执行`git pull`的时候自动更新的，因为`git pull`只会获取 submodules 所指向的 commit 的信息，它并不会获取 submodules 的代码，去更新代码你可以执行

```
git submodule update
```

如果 submodule 还没有被初始化，你还需要加上`--init`参数；如果任何 submodule 有它自己的 submodules，你还可以通过`--recursive`，它就会递归的初始化和更新所有嵌套的 submodules;

##### 如果你不运行这个命令会发生什么

如果你不执行这个命令，你 submodule 的代码仍然指向旧的 commit。当你执行`git status`时，就会展示`Changes not staged for commit`信息；如果你在 submodule 目录下执行`git status`，它就会输出`HEAD detached at <commit-hash>`. 这不是因为你对 submodules 下的代码有修改，是因为你没有更新 submodules 的代码。所以如果你想使用 submoduels，不要忘记及时更新；

### 让它操作起来更简单

git submodule 的命令太多太杂了，大家很有可能会忘记初始化或者更新你的 submodules；幸运的是，有一键执行 初始化/更新的命令：

```
git clone --recurse-submodules
```

```
git pull --recurse-submodules
```

```
git config --global alias.clone-all 'clone --recurse-submodules'
git config --global alias.pull-all 'pull --recurse-submodules'
```
