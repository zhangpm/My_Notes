---
title: Linux Note
mathjax: true
---
https://www.bilibili.com/video/av47009997/?p=25
# 文件操作
1. shell 提示符[用户@主机名 目录]提示符  root：#   用户：$
2. linux 命令输入规律：
   命令 [选项]（[参数]）[选项的值]（[参数的值]）
3. 常见选项：-h --help -字母，--单词
4. hwclock 硬件时间
5. 关机： shutdown,init,reboot
6. init 命令：切换系统运行级别，7个启动级别
   init 0: 0级别，即关机
   init 6：重启

## 目录结构
1. / 根分区
2. /etc 配置文件，启动关闭脚本：/etc/passwd,etc/init.d/network
3. /boot 启动加载文件
4. /home 用户
5. /var 可增长目录
6. /root 管理员 
7. /usr unix software resource
8. /bin 命令，二进制可执行文件
9. /sbin 系统命令 reboot等
10. /mnt 挂载点
11. /dev 设备文件 ：
    /dev/sda 硬盘
    一切皆文件

## 相对路径，绝对路径
1. . 表示当前目录； ..表示上级目录
   cd ./;  cd ../

## 文件和文件夹
1. 创建文件：touch a.txt
2. 创建目录：mkdir /tmp/test
3. 创建多个 -p 连同父目录一起创建
   eg. mkdir -p /a/b/c
4. cd !$ : 上一命令的最后一个参数
5. cd ../../
6. 查看文件：cat；vim；more；less
7. head -n 显示前3行，默认10行 
   tail -n 显示后n行； -f 动态显示数据， 查看日志
8. 文件的复制：cp 源文件 目标文件
9. 目录的复制：cp -r 。。。 递归执行
10. 蓝色显示：目录；浅蓝色：软链接； 普通色：文件
11. 删除 rm -rf 全部删除：-r递归，-f强制删除
12. 重命名：mv 文件名（目录） 新文件名（目录）

# p9-远程连接工具
1. xmanager
2. 查看linux ip地址： ifconfig
3. 安装ssh： 
   1. 更新软件源：sudo apt-get update ，sudo apt-get upgrade
   2. 安装ssh: sudo apt-get install -y openssh-server
4. XShell连接，填写用户名zhangpm，密码zhangpm123
5. Xstart, ubuntu安装xterm: sudo apt-get install -y xterm
   Xstart 命令选xterm
6. uname -m 查看系统版本

# p-10 vim使用
1. rpm 安装软件
2. 模式：
   1. 命令行模式：
   :w 保存；
   :q 退出
   :q! 强退
   :wq 保存退
   :wq! 强制保存退出
   : r修改单个文字
   :ZZ 保存
   :%s/this/that 每一行第一个this替换成that
   :%s/this/that/g 文本中所有this 替换成that

   2. 视图模式：ctrl V
       *多行注释：
     1. ctrl v
     2. 移动光标选中多行
     3. 按大写I
     4. 插入注释符号
     5. Esc
3. 搜索： “/”符号  n下找，N下找
   :noh 不高亮显示
4. :!+命令行命令
5. :set nu 显示行号
6. 行首 gg
7. 行尾 G
8. 自定义vim ： vim /root/.vimrc， 写入自定义，例如 set nu

# p-11
1. Xshell rz 命令上传文件到linux
   1. 安装rz： sudo apt-get install lrzsz
2. 文字乱码：win：gb2312； linux: utf-8
   转码：iconv -f gb2312 c.txt -o sss.txt
3. linux->windows, 串行，处理回车不同（换行符问题）
   解决：安装unix2dos.
   unix2dos b.txt

# p12-文件,用户，组
1. root:x:0:0:root:/root:/bin/bash
            UID            Shell
2. 密码： /etc/shadow
3. 添加用户：useradd -u 5003 zhangpm2
4. 修改当前用户密码：passwd
5. 非交互添加密码：echo 123456 | passwd --stdin root
6. linux 用户密码加密：sha512
7. usermod 更改用户模式
8. userdel 删除用户 -username
9. 添加组：groupadd XX
10. su 切换用户：

    ​	切换到root：su

    ​	切换到其他用户：su - zhangpm  （注意中间的短横，代表连带shell一同切换）
11. single user mode 单用户模式，破解更改root密码
    方法2：root passwd 中x密码占位符删掉，登录则不需密码
12. 权限：
    1. 文件：r w x 读 写 执行
    2. 目录：r读，w增删移动文件，x进入
    3. 修改权限：chmod
    4. chomd u+r 拥有者user加r权限
        g-w 组减去w权限
        o=x others覆盖赋值x权限
        a+x all加x权限
    5. chown 修改所属组 -R递归
    6. touch a.txt 创建a文件
    7. 只读文件可以强制改写
    8. echo aaa>>a.txt aaa追加到a.txt
    9. chattr +a 无法删除的文件 +i 无法追加
    10. lsattr a.txt 查看attr权限
    11. charttr -a 去除a权限

# p17软件包安装
1. 挂载：mount
2. ls(ll) /a/b |wc -l，查看行数
3. 源码安装：需要：开发工具，开发库
   1. 解压：**tar -jxvf** 文件名； gz结尾文件：tar -zxvf
   2. cd 文件名
   3. 检查安装环境：**./configure**：可能报错，需要安装部分包
   4. 编译：**make**
   5. 安装：**make install**
   6. 删除：在安装路径下：**make uninstall**，**make clean**

# P19
1. tar 命令。打包，压缩
   tar -cf（-cvf） a.tar foo bar 打包create tar
   tar -tvf archive.tar  ~list all files in tar
   tar -xf（-xvf） archive.tar   extract files from tar
2. c:create;v:verbose(详细);f:file;x:extract
3. tar --help | grep v 查看tar命令help里的“-v”
4. linux 拓展名不能代表类型
5. 查看文件类型：file 文件名（路径）
6. 解包：tar -xvf archive.tar -C PATH
7. du -h /boot/ 查看文件大小：disk usage
8. tar 打包+压缩：格式：.tar.gz 或 .tgz
   语法：tar -czvf newfile.tar.gz SOURCE
   -z, --gzip
   解压缩：tar -zxvf newfile.tar.gz -C PATH
   -C: change directory to
9.  bz2 : 另一种压缩格式：z改为j
10. zip 压缩
    zip a.zip FILE 压缩文件
    zip -r a.zip DIRECTION 压缩目录
    解压：unzip -d PATH

# P21 进程管理
1. 程序：静态； 进程：动态
2. 进程和线程：一个程序至少一个进程，一个进程至少一个线程
   进程之间内存独立；
   线程之间内存共享，高并发好，安全性差
3. 进程树：pstree：（ps：processes）
4. man ps 查看ps命令手册
5. ps -aux （a：all，u：user；x：执行命令）
6. ps STAT 状态：5种：
   R:RUNNING
   S: sleeping
   T: 暂停（ctrl z）
   (ctrl c 停止)
   Z：zombie僵尸状态
   D：不可中断
7. < 高优先级
   N 低优先级
   s 包含子进程
   l 多线程
   + 前台程序
8. top 动态查看进程 
 
 |PID| USER|     PR | NI  |  VIRT  |  RES  |  SHR| S | %CPU| %MEM|
 |-|-|-|-|-|-|-|-|-|-|
|进程号|用户||nice优先级||实际使用内存大小|进程状态（6条）||||

1. q退出, M 按内存排序，P 按CPU排序,<>翻页
2.  **kill 信号 PID** 停止（控制）进程； 信号：
* 1，HUP 重启 
* 2，INT = ctrl c 通知终止
* 9，kill 强行中断
* 19，stop = ctrl z
* eg. kill -9 [PID]
1.  killall 程序名
    或：pkill 程序名
2.  nice 优先级
    1. -20 ~ 19，高~低
    2. nice -n -5 vim a.txt 用优先级-5打开a.txt，优先级高，运行快
    3. renice -n 5 pid
3.  jobs列出所有后台进程
4.  后台变前台：fg + 后台进程序列号
5.  查看系统内存
    1. free -m
    2. buffer缓存读入/cache缓存待写入
    3. 存储结构：disk->buffer/cache->memory
6.  远程终端后台运行的方法：
    1. screen 安装，使用
    2. 敲screen
    3. 输入ctrl+a+d
    4. 显示detached
    5. 重新连接：
    6. screen ls 查看id
    7. screen -r id
    8. screen -wipe 清楚死去的窗口
    9. 方法2：nohub 命令 &

# P24 输入输出重定向
1. 文件描述符：
   * 0：STDIN, 标准输入
   * 1: STDOUT，标准输出
   * 2: STDERR，标准错误输出
2. 输出重定向：
   * \> 默认为1
   * eg: xxx >/dev/null  2>&1  
   * 解释：/dev/null:黑洞文件夹；>为输出重定向；
   * 1> 标准输出重定向
   * 2> 标准错误输出重定向
   * & 等同于
   * 2>&1：错误输出重定向等同于1
   * eg：ls /home 1>a.txt
   * 把ls的结果重定向给a.txt
   * eg: ls /home/ /homee 1>log.txt 2>err.txt
   * ls /home/ /homee > a.txt 2>&1
   * ls语句标准输出1重定向到a.txt，错误2输出重定向到“等同于1”，即也是a.txt
   * \> 和 >> 的区别：>>表示追加写入；\> 覆盖写入
4. 输入重定向：\<
   * eg: wc 命令，word count
   * 用法1：cat /etc/passwd | wc -l 行数统计
   * 用法2：`wc < /etc/passwd` 显示行；单词；大小
   * `<<EOF` 
   * `cat > a.txt <<EOF` 输入以EOF结尾的文字，输入到a.txt, 并cat输出
5. 管道 `|` 输出作为输入
   * eg: cat /etc/passwd | more
6. `tee`标准输出的内容写到文件中
   * cat /etc/passwd | tee a.txt
   * 跟`>`区别：>不显示

# P25 文件查找命令
1. 