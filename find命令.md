+ find < path > < expression > < cmd >
> path： 所要搜索的目录及其所有子目录。默认为当前目录。

> expression： 所要搜索的文件的特征。

> cmd： 对搜索结果进行特定的处理

> 如果什么参数也不加，find默认搜索当前目录及其子目录，并且不过滤任何结果（也就是返回所有文件），将它们全都显示在屏幕上。
-----------------------
+ -name 按照文件名查找文件
   + find /dir -name filename  在/dir目录及其子目录下面查找名字为filename的文件
      + find ./data_jjw/ -name 'file.txt' -print（打印）
   + find . -name "*.c" 在当前目录及其子目录（用“.”表示）中查找任何扩展名为“c”的文件
-----------------------
+ -perm 按照文件权限来查找文件。
   + find . -perm 755 –print 在当前目录下查找文件权限位为755的文件，即文件属主可以读、写、执行，其他用户可以读、执行的文件
-----------------------
+ -prune 使用这一选项可以使find命令不在当前指定的目录中查找，如果同时使用-depth选项，那么-prune将被find命令忽略。
   + find /apps -path "/apps/bin" -prune -o –print 在/apps目录下查找文件，但不希望在/apps/bin目录下查找
   + find /usr/sam -path "/usr/sam/dir1" -prune -o –print 在/usr/sam目录下查找不在dir1子目录之内的所有文件
-----------------------
+ -depth：在查找文件时，首先查找当前目录中的文件，然后再在其子目录中查找。
   + find / -name "CON.FILE" -depth –print 它将首先匹配所有的文件然后再进入子目录中查找
-----------------------
+ -user 按照文件属主来查找文件。
   + find ~ -user sam –print 在$HOME目录中查找文件属主为sam的文件
-----------------------
+ -group 按照文件所属的组来查找文件。
   + find /apps -group gem –print 在/apps目录下查找属于gem用户组的文件
-----------------------
+ -mtime -n +n 按照文件的更改时间来查找文件， -n表示文件更改时间距现在n天以内，+n表示文件更改时间距现在n天以前
   + find / -mtime -5 –print 在系统根目录下查找更改时间在5日以内的文件
   + find /var/adm -mtime +3 –print 在/var/adm目录下查找更改时间在3日以前的文件
-----------------------
+ -nogroup 查找无有效所属组的文件，即该文件所属的组在/etc/groups中不存在。
   + find / –nogroup -print
-----------------------
+ -nouser 查找无有效属主的文件，即该文件的属主在/etc/passwd中不存在。
   find /home -nouser –print
-----------------------
+ -type 查找某一类型的文件，
   + b - 块设备文件。
   + d - 目录。
   + c - 字符设备文件。
   + p - 管道文件。
   + l - 符号链接文件。
   + f - 普通文件。 
      + find /etc -type d –print 在/etc目录下查找所有的目录
      + find . ! -type d –print 在当前目录下查找除目录以外的所有类型的文件
      + find /etc -type l –print 在/etc目录下查找所有的符号链接文件
-----------------------
+ 一些常用命令
   1. find . -type f -exec ls -l {} \;
   查找当前路径下的所有普通文件，并把它们列出来。
   2. find logs -type f -mtime +5 -exec rm {} \;
   删除logs目录下更新时间为5日以上的文件。
   3.find . -name "*.log" -mtime +5 -ok rm {} \;
   删除当前路径下以。log结尾的五日以上的文件，删除之前要确认。
   4. find ~ -type f -perm 4755 -print
   查找$HOME目录下suid位被设置，文件属性为755的文件打印出来。
   说明： find在有点系统中会一次性得到将匹配到的文件都传给exec，但是有的系统对exec的命令长度做限制，就会报：”参数列太长“，这就需要使用xargs。xargs是部分取传来的文件。
   5. find / -type f -print |xargs file
   xargs测试文件分类
   6. find . -name "core*" -print|xargs echo " ">/tmp/core.log
   将core文件信息查询结果报存到core。log日志。
   7. find / -type f -print | xargs chmod o -w
   8. find . -name * -print |xargs grep "DBO"
