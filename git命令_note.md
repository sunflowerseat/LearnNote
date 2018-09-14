---
Author : Fancy
Time : 2018-09-05 16:08:19
---

# git 命令汇总

```
git clone git@git.archtab.com:Panama_ios_v1.1
git clone git@git.archtab.com:Panama_android_v2.2
```



关键字 ： `回滚 | reverse `

```
  $ git reset --hard HEAD^         回退到上个版本
  $ git reset --hard HEAD~3        回退到前3次提交之前，以此类推，回退到n次提交之前
  $ git reset --hard commit_id     退到/进到 指定commit的sha码
```


关键字 ：`克隆 | clone`

```
git clone xxx
```



关键字 ： `日志 | log`

```
git log
```

关键字 ： `撤销 | revert`

```
git revert HEAD: 撤销最近的一个提交.
```





windows 在某个文件夹打开cmd

shift + 右键 ->  W (在此处打开命令行窗口) -> 回车(Enter)



git 不用每次输密码

执行 `git config --global credential.helper store`

然后正常git push 步骤执行一次 ，以后不用再输密码



一键push

```
git add . && git commit -m 'xxx' && git pull && git push
```

