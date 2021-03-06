# 一, Linux 目录结构

## 1.1 基本介绍

+ Linux 的文件系统采用层级式的树状目录结构, 在此结构中的最上层是根目录 "/" , 然后在此目录下在创建其他目录
+ 深刻理解Linux树状文件目录是非常重要的
+ 记住一句话: 在Linux世界中, 一切皆文件



## 1.2 具体的目录结构

+ /bin (/usr/bin , /usr/local/bin)

  是Binary的缩写, 这个目录存放着经常使用的命令

+ /sbin (/usr/sbin , /usr/local/sbin)

  s就是Super User的意思, 这里存放的是系统管理员使用的系统管理程序

+ /home

  存放普通用户的主目录, 在Linux中每个用户都有一个自己的目录, 一般该目录名是以用户的账号命名

+ /root

  该目录为系统管理员, 也称作超级权限者的用户主目录

+ /lib

  系统开机所需要最基本的动态连接共享库, 起作用类似于Windows里的DLL文件, 几乎多有的应用程序都需要用到这些共享库

+ /lost+found

  这个目录一般情况下是空的, 当系统非法关机后, 这里会存放一些文件

+ etc

  所有的系统管理所需的配置文件和子目录, 比如安装MySql数据库 my.conf

+ /usr

  这是一个非常重要的目录, 用户的很多应用程序和文件都放在这个目录下, 类似与 Windows 下的program files目录

+ /boot

  存放的是启动Linux时使用的一些核心文件, 包括一些连接文件以及镜像文件

+ /proc

  这个目录是一个虚拟的目录, 它是系统内存的映射, 访问这个目录来获取系统信息

+ /srv

+ /sys

+ /tmp

+ /dev

+ /media

+ /mnt

+ /opt

  这是给主机额外安装软件所摆放的目录, 如安装ORACLE数据库就可放在该目录下, 默认为空

+ /usr/local

+ var

+ /selinux [security-enhanced linux]



# 二, vi 和 vim 指令

## 2.1 介绍

Linux 系统内置 vi 文本编辑器

vim 具有程序编辑的能力, 可以看作是vi的增强版本,可以主动的以字体颜色辨别语法的正确性, 方便程序设计. 代码补完, 编译及错误跳转等方便编程的功能特别丰富, 在程序员中被广泛使用



## 2.2 常用三种模式

+ **正常模式**

  以vim打开一个档案就直接进入一般模式(默认模式). 在这模式下, 你可以使用 [上下左右] 案件来移动光标, 你可以使用 [删除字符] 或 [删除整行] 来处理档案内容, 也可以使用 [复制粘贴] 来处理文件数据

+ **插入模式**

  按下 i, I, o, O, a, A, r, R 等任意一个字母之后才进入编辑模式, 一般来说按 i 即可

+ **命令行模式**

  输入esc 再输入 : 进入命令行模式, 在这个模式下, 可以提供你相关指令, 完成读取, 存盘, 替换, 离开 vim, 显示行号等动作



## 2.3 各种模式的互相切换

![2](https://raw.githubusercontent.com/blank-wz/typoraimage/main/images/2022/03/23/ae9a91db21271b6124b178c33f4db5ac-2-6d2657.png)



## 2.4 vi 和 vim 快捷键

+ 拷贝当前行 yy, 拷贝当前行向下的5行 5yy, 并输入p粘贴
+ 删除当前行 dd, 删除当前行向下的5行 5dd
+ 在文件中查找某个单词[命令行下 /关键字, 回车 查找, 输入n就是查找下一个]
+ 设置文件的行号, 取消文件的行号. 命令行下 :set nu 和 :set nonu
+ 跳转到文档的最末行 G, 最首行 gg
+ 在一个文件中输入 "hello", 然后撤销这个动作 u



# 三, 文件目录指令



+ pwd 

  功能描述: 显示当前工作目录的绝对路径

+ ls [选项]

  功能描述: 查看文件信息

  -a : 显示当前目录所有的文件和目录, 包括隐藏的

  -l : 以列表的方式显示信息 或者 直接 ll

+ cd [参数]

  功能描述: 切换到指定目录

  cd ~ 或者 cd : 回到自己的家目录, 比如你是root

  cd .. : 回到当前目录的上一级目录

+ mkdir [选项]

  功能描述: 创建目录

  -p : 创建**多级目录** 如: mkdir -p /home/animal/tiger

+ rmdir [选项]

  功能描述: 删除**空目录**

  -rf : 删除非空目录 (递归删除)

+ touch

  功能描述: 创建空文件

+ rm [选项]

  功能描述: 删除文件或目录

  -r : 递归删除整个文件夹

  -f : 强制删除不提示

  -rf : 强制删除整个文件夹

+ cp [选项] source dest

  功能描述: 拷贝文件到指定目录

  -r : 递归复制整个文件夹

  \cp -r /home/bbb /opt  不提示是否覆盖地复制

+ mv

  功能描述: 移动文件与目录或者重命名

  mv oldNameFile newNameFile  重命名

  mv /temp/moveFile /targetFolder  移动文件到target目录

  mv /temp/moveFile /targetFolder/newFileName  移动并且重命名

  mv /temp/moveFolder /targetFolder 移动整个目录

  mv /temp/moveFolder /targetFolder/newFolderName 移动整个目录并改名

+ cat [选项]

  功能描述: 查看文件内容

  -n 查看并显示行号

  *使用细节*: cat 只能浏览文件, 而不能修改文件, 为了浏览方便, 一般会带上管道命令 | more

  cat -n /etc/profile | more [进行交互]

+ more

  功能描述: 基于VI编辑器的文本过滤器, 它以全屏幕的方式按页显示文本的内容, more 指令中内置了若干个快捷键(交互指令)  

  more 要查看的文件

+ less

  

