# 1. Git 工作机制和代码托管中心

## 1.1 工作机制

**工作区 (写代码)** - `git add` -> **暂存区(临时存储)** - `git commit` -> **本地库(历史版本)** - `push` -> **远程库**



## 1.2 Git 和代码托管中心

代码托管中心是基于网络服务器的远程代码仓库, 一般我们简单称为<font color='red'>远程库</font>

+ 局域网
  - GitLab
+ 互联网
  + GitHub
  + Gitee 码云



# 2. 常用命令

| 命令名称                             | 作用           |
| ------------------------------------ | -------------- |
| git config --global user.name 用户名 | 设置用户签名   |
| git config --global user.email 邮箱  | 设置用户签名   |
| git init                             | 初始化本地库   |
| git status                           | 查看本地库状态 |
| git add 文件名                       | 添加到暂存区   |
| git commit -m "日志信息" 文件名      | 提交到本地库   |
| git reflog                           | 查看历史记录   |
| git reset --hard 版本号              | 版本穿梭       |

## 2.1 设置用户签名

+ 说明:
  + 签名的作用是区分不同操作者的身份. 用户的签名信息是在每个版本的提交信息中能够看到, 以此确认本次提交是谁做的. <font color='red'>Git 首次安装必须设置一下用户签名, 否则无法提交代码</font>
  + 这里设置用户签名和将来登录 GitHub 或其他代码托管中心的账号没有任何关系



## 2.2 初始化本地库

**git init**



## 2.3 查看本地库状态

**git status**



## 2.4 添加暂存区

**git add 文件名**

**git rm --cached 文件名** - 从暂存区删除



## 2.5 提交本地库

**git commit -m "日志信息" 文件名**

`若不写-m "日志信息,接下来会自动进入文本编辑"`

+ 查看日志

  + **git reflog**

    ````git
    1290a3a (HEAD -> master) HEAD@{0}: reset: moving to 1290a3a
    b89bc68 HEAD@{1}: commit: third commit
    1290a3a (HEAD -> master) HEAD@{2}: commit: second commit
    fa663ac HEAD@{3}: commit (initial): first commit
    ````

    

  + **git log**  - 详细日志

    ````
    commit fa663aca5de227cee39c6d65801a1ae974fadb78 (HEAD -> master)
    Author: blank_wz <blank_wz@outlook.com>
    Date:   Sun Oct 10 21:41:00 2021 +0800
    ````

    



## 2.6 修改文件

**vim 文件名**

**cat 文件名**

**tail -n 数字 文件名**  - 查看文件倒数几行



## 2.7 版本穿梭

**git reset --hard 版本号**



# 3. 分支操作

| 命令名称            | 作用                         |
| ------------------- | ---------------------------- |
| git branch 分支名   | 创建分支                     |
| git branch -v       | 查看分支                     |
| git checkout 分支名 | 切换分支                     |
| git merge 分支名    | 把指定的分支合并到当前分支上 |

![3](https://raw.githubusercontent.com/blank-wz/typoraimage/main/images/2022/03/23/6175833afb5a694ae54fa9b3f94eeada-3-819070.png)

## 3.1 查看分支

**git branch -v**

````
$ git branch -v
* master 1290a3a second commit
````



## 3.2 创建分支

**git branch 分支名**

````
blank@blank MINGW64 /e/GitDemo (master)
$ git branch hot-fix

blank@blank MINGW64 /e/GitDemo (master)
$ git branch -v
  hot-fix 1290a3a second commit
* master  1290a3a second commit
````



## 3.3 切换分支

**git checkout 分支名**

````
$ git checkout hot-fix
Switched to branch 'hot-fix'

$ git branch -v
* hot-fix 1290a3a second commit
  master  1290a3a second commit
````



## 3.4 合并分支

**git merge 分支名**

````
blank@blank MINGW64 /e/GitDemo (master)
$ git merge hot-fix
Updating 91f4399..824629a
Fast-forward
 hello.txt | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)
````



## 3.5 冲突合并

+ 冲突原因:

  合并分支时, 两个分支在<font color='red'>同一个文件的同一个位置</font>有两套完全不同的修改. Git 无法替我们决定使用哪一个, 必须<font color='red'>人为决定</font>新代码内容



````
blank@blank MINGW64 /e/GitDemo (master)
$ git merge hot-fix
Auto-merging hello.txt
CONFLICT (content): Merge conflict in hello.txt
Automatic merge failed; fix conflicts and then commit the result.

blank@blank MINGW64 /e/GitDemo (master|MERGING)
$ git status
On branch master
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)
        both modified:   hello.txt

no changes added to commit (use "git add" and/or "git commit -a")

blank@blank MINGW64 /e/GitDemo (master|MERGING)
$ git commit -m "merge test" hello.txt  # 此时不能带文件名
fatal: cannot do a partial commit during a merge.

blank@blank MINGW64 /e/GitDemo (master|MERGING)
$ git commit -m "merge test"
[master 704cda5] merge test

blank@blank MINGW64 /e/GitDemo (master)
````



# 4. Git 团队协作机制

## 4.1 **团队协作**

![4](https://raw.githubusercontent.com/blank-wz/typoraimage/main/images/2022/03/23/5ac429fb43432a461d9118f48c5a8e21-4-f75bd0.png)



## 4.2 **跨团队协作**

![5](https://raw.githubusercontent.com/blank-wz/typoraimage/main/images/2022/03/23/ad25fd9faf94d0283da97a8178b844a6-5-4be3ef.png)





# 5. GitHub



























