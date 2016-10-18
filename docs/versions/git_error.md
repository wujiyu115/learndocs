git 问题记录
===========================


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