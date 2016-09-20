Git 备忘录
=================


https://bingohuang.gitbooks.io/progit2/content/

https://leohxj.gitbooks.io/learning-git/content/appendix/learning-assets.html

## Git 常用操作:
### 操作
+ git删除未跟踪文件
```shell
# 删除 untracked files
git clean -f
# 连 untracked 的目录也一起删掉
git clean -fd
# 连 gitignore 的untrack 文件/目录也一起删掉 （慎用，一般这个是用来删掉编译出来的 .o之类的文件用的）
git clean -xfd
# 在用上述 git clean 前，墙裂建议加上 -n 参数来先看看会删掉哪些文件，防止重要文件被误删
git clean -nxfd
git clean -nf
git clean -nfd
```


+ git回滚远程版本
```shell
    git log
    git reset --soft ${commit-id}
    git stash
    git push -f
```


+ git 丢弃更改
```shell
git reset --hard
```

### 下面是一张常用命令的图解

![](http://farwmarth.com/wp-content/2010072023345292.png)

[![image](http://farwmarth.com/wp-content/uploads/2013/08/image_thumb9.png "image")](http://farwmarth.com/wp-content/uploads/2013/08/image9.png) 上面的部份: 以 左上方红色的remote repository(从远端 clone 一份到 local端) 或 黄色区块的local repository 为起点, 来看此区块.
下面的部份: 由 Local repository 开始看, 主要是说明 做 local branch 的流程.
### github图解
https://help.github.com/articles/set-up-git/

![github.png](/_static/github.jpg)


## 概念
### submodule
#### git submodule update并不会将submodule切到任何 branch
有些时候你需要对submodule做一些修改，很常见的做法就是切到submodule的目录，然后做修改，然后commit和push。
这里的坑在于，默认git submodule update并不会将submodule切到任何branch，所以，默认下submodule的HEAD是处于游离状态的(‘detached HEAD’ state)。所以在修改前，记得一定要用git checkout master将当前的submodule分支切换到master，然后才能做修改和提交。

#### git submodule下还有submodule
```shell
git submodule foreach git submodule update
```

#### 删除submodule
```shell
git rm --cached WeChat-Cloud-Robot
#对于显性定义的子模组，还要删除 .gitmodules 文件和 .git/config 文件中的相关条目。
```

### 只checkout部分git文件
```shell
git remote add -f origin <url>
git config core.sparsecheckout true
echo "sublime_windows" >> .git/info/sparse-checkout
echo "sublime_linux" >> .git/info/sparse-checkout
```

###   捋捋这些配置
####  .gitconfig
git的全局配置文件.
```yaml
[user]
    email = xxx@gmail.com
    name = xxx
[push]
    default = simple
[credential]
    helper = store
```
####  .git-credentials
这个也是git的push时不用多次输入密码的配置.
官方说明:<https://git-scm.com/docs/git-credential-store>
先将credential配置到git全局配置
`git config --global credential.helper store`
在`%HOME%` 目录下创建一个`  .git-credentials`输入
```
https://username@gmail.com:pwd@git.coding.net
```
####  _netrc
_netrc这个配置是用来自动化ftp上传下载的. 说明:<http://www.mavetju.org/unix/netrc.php> .需要600权限`chmod 600 .netrc.` 一般只要用machine段就可以了.常用仓库如下配置在push时不用再输入密码.
```
machine github.com
login username
password password

machine bitbucket.org
login username
password password

machine gitcafe.com
login username
password password
```
####  known_hosts
ssh会把你每个你访问过计算机的公钥(public key)都记录在~/.ssh/known_hosts.当下次访问相同计算机时,openssh会核对公钥.
```
github.com,192.30.252.128 ssh-rsa xxx(key)
git.coding.net,61.179.108.67 ssh-rsa  xxx(key)
```

### gitflow
http://nvie.com/posts/a-successful-git-branching-model/

### 另一种工作流
https://segmentfault.com/q/1010000000181403

```shell
#git支持很多种工作流程，我们采用的一般是这样，远程创建一个主分支，本地每人创建功能分支，日常工作流程如下：
#去自己的工作分支
git checkout work
#工作....
#提交工作分支的修改
git commit -a
#回到主分支
git checkout master
#获取远程最新的修改，此时不会产生冲突
git pull
#回到工作分支
git checkout work
#用rebase合并主干的修改，如果有冲突在此时解决
git rebase master
git add .
git rebase --continue
#回到主分支
git checkout master
#合并工作分支的修改，此时不会产生冲突。
git merge work
#提交到远程主干
git push
#这样做的好处是，远程主干上的历史永远是线性的。每个人在本地分支解决冲突，不会在主干上产生冲突
```

### 同一台电脑有2个github账号

比如我服务器上模拟的2个用户
```shell
#monkeysuzie@gmail.com   我在gitlab的第一个账号suzie
host gitlab.zjut.com
    hostname gitlab.zjut.com
    Port 65095
    User suzie
    IdentityFile /home/suzie/.ssh/id_rsa
#  我在gitlab的第2个账号test
host gitlab-test.zjut.com
    hostname gitlab.zjut.com
    Port 65095
    User test
    IdentityFile /home/suzie/.ssh/id_rsa_second
#837368104@qq.com 我在github的账号
host github-osteach.com
    hostname github.com
    Port 22
    User osteach
    IdentityFile /home/suzie/.ssh/id_rsa_second
```
这种情况下，需要几点注意

1.remote pull push的时候有问题，因为要设置邮箱问题了 pull的时候识别的是邮箱，2个github账号，2个邮箱，我们自然不能使用global的user.email了

取消global
```shell
git config --global --unset user.name
git config --global --unset user.email
```

2.设置每个项目repo的自己的user.email
```shell
git config  user.email "xxxx@xx.com"
git config  user.name "suzie"
```
之后push pull就木有问题了


## 工具
### Eclipse EGit

[http://blog.csdn.net/luckarecs/article/details/7427605](http://blog.csdn.net/luckarecs/article/details/7427605)

![](/_static/egit.jpg)

##  问题

+ 错误提示：fatal: remote origin already exists.
```
git remote rm origin
git remote add origin git@github.comxxx.git
```

+ git push origin master 错误提示：error: failed to push som refs to
```
git pull origin master //先pull 下来 再push 上去
```

+ Cannot list the available branches. Reason: git+ssh://git@[host]:[...]/[...].git: Auth fail
```
解决方法就是把$HOME/.ssh在原路径下复制一个，改名为ssh就好了。Windows里也是一样,这样就直接可以图形上push了
```

+ fatal:remote error:You can't push to git://github.com/user_name/user_repo.git
+ fatal: could not read Username for 'https://github.com': Invalid argument
```
git remote rm origin
git remote add origin git@github.com:user_name/user_repo.git
git push origin
如果在git clone的时候用的是git://github.com:xx/xxx.git 的形式,那么就会出现这个问题，因为这个protocol是不支持push的
```

+ Permission denied (publickey)
```
ssh-keygen -t rsa -C "wujiyu@163.com"  -f "github_rsa" 生成的配置文件名与默认的不符
更改ssh_config文件指定密钥文件名
Host github.com
StrictHostKeyChecking no
UserKnownHostsFile=/dev/null
IdentityFile=~/.ssh/github_rsa
```

+ cannot do a partial commit during a merge
原因是merge有冲突,手动解决后如下提交即可
```
git commit -a
```
