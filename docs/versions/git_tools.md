git 工具
=======================

## ubuntu合并对比工具
```shell
#安装meld
sudo apt-get install meld
```

编辑`~/.gitconfig`

```shell
[difftool]
    prompt = false
[mergetool]
    prompt = false
[diff]
    tool = meld
[merge]
    tool = meld

```

使用
```shell
git difftool 文件名
git megretool
```