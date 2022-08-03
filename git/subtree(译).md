(本文翻译自 git 源码中的帮助文档)
名称
——————
git-subtree —— 合并子树 & 分割仓库成子树

概要
——————
[语法]
`git subtree [<options>] -P <prefix> add <local-commit>` 分割仓库成子树？
'git subtree' [<options>] -P <prefix> add <repository> <remote-ref> 添加子树
'git subtree' [<options>] -P <prefix> merge <local-commit>
'git subtree' [<options>] -P <prefix> split [<local-commit>]
'git subtree' [<options>] -P <prefix> pull <repository> <remote-ref>
'git subtree' [<options>] -P <prefix> push <repository> <refspec>

描述
——————

Subtrees 允许子项目以子目录的形式包含在主项目中，并可以选择是否包含子项目的历史信息；

例如，你可以将一个库的源码作为你应用的一个子目录；

Subtrees 和 做同样工作的 Submodules 并不混淆；与 Submodules 不同的是，Subtrees 不需要额外的构造，而 Submodules 还需要 .gitmodules 和 gitlinks，并不需要主项目的开发者做额外的工作或者了解 subtree 是如何工作的。一个子树就仅仅是一个子目录，就如同项目中其他的子目录一样，可以与主项目一起提交、分支和合并，以任何你想要的方式。

也不需要对子树的合并策略感到困惑。最主要的区别在于，除了合并另外一个项目作为子目录以外，你也可以将你项目中的一个子目录完全抽离成一个独立的项目，并且他的历史记录依然存在；不同于子树的合并策略，你可以在这两个操作之间来回执行；如果这个独立的库更新了，你可以自然而然地把这些变更合并到自己的仓库，你也也可以“分割”子树的变更并把它们合并回它的远端项目；

例如，如果一个项目里的某个模块，你发现它在很多地方都有用到，就可以把它和它的整个的 git history 抽离出来，并作为一个独立的仓库去发布，它的 git history 不会和这个项目的 git history 混合；

[提示]
为了保持你的提交信息是洁净的，我们推荐人们尽可能地分割他们耦合了子树和主项目的提交记录。这也就是说，如果你做了一个变更，这个变更既包含到主项目也包含到子树，那就提交两次；这样，当你要分割这个库的 commit 记录时，它们 commit message 仍然是有意义的；不过如果这对你不重要就不必要这样做。在稍后将其拆分成子项目时，`git subtree`只会简单将 commit 中那些与子树无关的改动忽略掉；

命令
——————

add <local-commit>::

add <repository> <remote-ref>
通过导入 <repository> 和 <remote-ref> 给定的<local-commit>的内容，来创建子树，并添加在 <prefix> 目录下；一个新的 commit 记录会被自动创建，???

merge <local-commit>::
合并最近的变更到 <local-commit> 到 <prefix> 目录下的子树。和正常的`git merge`一样，`git subtree merge`操作不会清除你本地的变更，它只是会合并这些变更到本地的 head; 带上`--squash`参数，可以指创建一个 commit 记录，它包含了所有的历史变更，而不是把整个 `subtree` 的历史合入进来；

- 如果你使用`--squash`，

split [<local-commit>]
从<local-commit> 或者 HEAD 中的 <prefix> 目录下的子树，抽离一个新的，带有完成变更历史的项目；这个新的历史记录只包含了有<prefix>的 commit（包含影响<prefix>的 merges），并且每一个 commits 中的<prefix>内容的变更的根目录都变成了以项目为根目录而不是子目录 ？？？，因此，这个新的被创建的历史记录用来作为单独 git 仓库去导出的时候是很合适的；

- 在分离成功之后，一个新的 commit ID 会被打印出来，它与新创建的树的 HEAD 对应，你可以随心所欲的去用；

重复的 split 命令（有相同 git history）会生成相同的 commit ID,只要 split 命令携带的参数是相同的，比如--annotate；正因为如此，如果你添加了一些新的 commits，然后再去 split，这些新的 commits 会被添加到当前 commits 历史的顶端，；

options for 'add' AND 'merge'、'pull'、'split --rejion'、'push --rejion'

--squash::

使用这个参数可以有效减少杂乱无章的 git log 信息。大家很少想去看 v1.0 - v1.1 之间的每一个 change log，假如说它们中间没有一个临时版本作为过渡的话；
使用`--squash`也可以避免很多问题，比如一个项目上的多个子项目，这些子项目有时间上重复的 commits，或者是它移除了又被重新添加进来；在这样的例子里，兼并两者的历史记录是没有任何意义的，因为某个 commit 具体是属于哪个 subtree 是不清晰的；
