Git 备忘录
=================




### 下面是一张常用命令的图解

![](http://farwmarth.bestnewbee.com/images/2010072023345292.png)


### 只checkout部分git文件
```shell
git remote add -f origin <url>
git config core.sparsecheckout true
echo "sublime_windows" >> .git/info/sparse-checkout
echo "sublime_linux" >> .git/info/sparse-checkout
```

### 推送git项目到多个远程仓库
```
如果已经配置好了ssh验证方式，在开源中国的git托管也可以使用同一个的key，然后打开github项目中中的.git/config文件 在[remote "origin"]节点的原始url下面直接添加开源中国git中对应项目的ssh地址即可，例如：

[remote "origin"]
    url = git@github.com:codepiano/pull-all-git-project.git
    url = git@git.oschina.net:codepiano/pull-all-git-project.git
    fetch = +refs/heads/*:refs/remotes/origin/*
当然，使用命令行也可以直接添加，命令格式如下：

git remote set-url --add origin git@git.oschina.net:codepiano/pull-all-git-project.git
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



### git工作流

#### 工作流
```shell
#提交流程
git stash
git pull  -r
git stash pop
#解决冲突
git commit -m 'xxx'
git push
#如果commit之后push发现有人提交
git reset --soft HEAD^
git pull  -r
```

### 工作流二
```shell
git commit
git pull -r
解决冲突
git pull -r
git push
```

### 工作流三
```shell
1.git pull -r
2.A本地修改冲突的话git stash，B已经commit的修改冲突git mergetool
3.A再次.git pull -r
4.A git stash pop stash{0}
5.A 有冲突再合并git mergetool
```

### git 被push -r 丢失日志
```shell
git reflog
git log -g
git cherry-pick commit_id
#https://www.borfast.com/blog/2014/10/19/how-to-undo-a-git-push---force-and-undelete-things/
```

### 配置禁止 git push -r
```shell
git config --global receive.denyNonFastForwards true
```

### 不commit情况下pull
```shell
git stash
git pull
git stash pop
git stash drop stash{1}
git stash clear
```

### git stash clear 后恢复
```shell
git fsck --lost-found
git show 8dd73fa8d14880182f11e24dc10bca570b6127d7
git merge 8dd73fa8d14880182f11e24dc10bca570b6127d7
```
