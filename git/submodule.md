# Git submodule

git submodule 允许 一个 git repo 将另外一个 git repo 分开;

使用场景：您希望将两个项目看成是独立的，但是又希望能够在一个项目中使用另外一个项目的(源)代码；

使用方式

```
git submodule add url
```

默认情况下，submodule 将会被添加进与仓库名相同的目录下，同时会在根目录下新增一个 `.gitmodules` 配置文件，用于记录 submodule 的仓库信息；如果你有多个 submodules，这个配置文件里将会记录多条配置信息；这个配置信息同样也会应用于 当别人 git clone 这个项目时，知道该去哪里 clone submodule；
