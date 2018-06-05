# linux 常用命令(一)
#### 系统信息命令

arch 显示机器的处理器架构(1) <br/>
uname -m 显示机器的处理器架构(2) <br/>
uname -r 显示正在使用的内核版本 <br/>
dmidecode -q 显示硬件系统部件 - (SMBIOS / DMI) <br/>
hdparm -i /dev/hda 罗列一个磁盘的架构特性 <br/>
hdparm -tT /dev/sda 在磁盘上执行测试性读取操作 <br/>
cat /proc/cpuinfo 显示CPU info的信息 <br/>
cat /proc/interrupts 显示中断 <br/>
cat /proc/meminfo 校验内存使用 <br/>
cat /proc/swaps 显示哪些swap被使用 <br/>
cat /proc/version 显示内核的版本 <br/>
cat /proc/net/dev 显示网络适配器及统计 <br/>
cat /proc/mounts 显示已加载的文件系统 <br/>
lspci -tv 罗列 PCI 设备 <br/>
lsusb -tv 显示 USB 设备 <br/>
date 显示系统日期 <br/>
cal 2007 显示2007年的日历表 <br/>
date 041217002007.00 设置日期和时间 - 月日时分年.秒 <br/>
clock -w 将时间修改保存到 BIOS <br/>
<hr>

####  系统的关机、重启以及登出
shutdown -h now 关闭系统(1) <br/>
init 0 关闭系统(2) <br/>
telinit 0 关闭系统(3) <br/>
shutdown -h hours:minutes & 按预定时间关闭系统 <br/>
shutdown -c 取消按预定时间关闭系统 <br/>
shutdown -r now 重启(1) <br/>
reboot 重启(2) <br/>
logout 注销<br/>
<hr>

#### 文件和目录 
cd /home 进入 '/ home' 目录' <br/>
cd .. 返回上一级目录 <br/>
cd ../.. 返回上两级目录 <br/>
cd 进入个人的主目录 <br/>
cd ~user1 进入个人的主目录 <br/>
cd - 返回上次所在的目录 <br/>
pwd 显示工作路径 <br/>
ls 查看目录中的文件 <br/>
ls -F 查看目录中的文件 <br/>
ls -l 显示文件和目录的详细资料 <br/>
ls -a 显示隐藏文件 <br/>
ls *[0-9]* 显示包含数字的文件名和目录名 <br/>
tree 显示文件和目录由根目录开始的树形结构(1) <br/>
lstree 显示文件和目录由根目录开始的树形结构(2) <br/>
mkdir dir1 创建一个叫做 'dir1' 的目录' <br/>
mkdir dir1 dir2 同时创建两个目录 <br/>
mkdir -p /tmp/dir1/dir2 创建一个目录树 <br/>
rm -f file1 删除一个叫做 'file1' 的文件' <br/>
rmdir dir1 删除一个叫做 'dir1' 的目录' <br/>
rm -rf dir1 删除一个叫做 'dir1' 的目录并同时删除其内容 <br/>
rm -rf dir1 dir2 同时删除两个目录及它们的内容 <br/>
mv dir1 new_dir 重命名/移动 一个目录 <br/>
cp file1 file2 复制一个文件 <br/>
cp dir/* . 复制一个目录下的所有文件到当前工作目录 <br/>
cp -a /tmp/dir1 . 复制一个目录到当前工作目录 <br/>
cp -a dir1 dir2 复制一个目录 <br/>
ln -s file1 lnk1 创建一个指向文件或目录的软链接 <br/>
ln file1 lnk1 创建一个指向文件或目录的物理链接 <br/>
<hr>

#### 文件搜索
find / -name file1 从 '/' 开始进入根文件系统搜索文件和目录 <br/>
find / -user user1 搜索属于用户 'user1' 的文件和目录 <br/>
find /home/user1 -name \*.bin 在目录 '/ home/user1' 中搜索带有'.bin' 结尾的文件<br/> 
find /usr/bin -type f -atime +100 搜索在过去100天内未被使用过的执行文件 <br/>
find /usr/bin -type f -mtime -10 搜索在10天内被创建或者修改过的文件 <br/>
find / -name \*.rpm -exec chmod 755 '{}' \; 搜索以 '.rpm' 结尾的文件并定义其权限 <br/>
find / -xdev -name \*.rpm 搜索以 '.rpm' 结尾的文件，忽略光驱、捷盘等可移动设备 <br/>
locate \*.ps 寻找以 '.ps' 结尾的文件 - 先运行 'updatedb' 命令 <br/>
whereis halt 显示一个二进制文件、源码或man的位置 <br/>
which halt 显示一个二进制文件或可执行文件的完整路径 <br/>
<hr>

#### 磁盘空间 
df -h 显示已经挂载的分区列表 <br/>
ls -lSr |more 以尺寸大小排列文件和目录 <br/>
du -sh dir1 估算目录 'dir1' 已经使用的磁盘空间' <br/>
du -sk * | sort -rn 以容量大小为依据依次显示文件和目录的大小 <br/>
rpm -q -a --qf '%10{SIZE}t%{NAME}n' | sort -k1,1n 以大小为依据依次显示已安装的rpm包所使用的空间 (fedora, redhat类系统) <br/>
dpkg-query -W -f='${Installed-Size;10}t${Package}n' | sort -k1,1n 以大小为依据显示已安装的deb包所使用的空间 (ubuntu, debian类系统) <br/>
<hr>

#### 用户和群组 
groupadd group_name 创建一个新用户组 <br/>
groupdel group_name 删除一个用户组 <br/>
groupmod -n new_group_name old_group_name 重命名一个用户组 <br/>
useradd -c "Name Surname " -g admin -d /home/user1 -s /bin/bash user1 创建一个属于 "admin" 用户组的用户 <br/>
useradd user1 创建一个新用户 <br/>
userdel -r user1 删除一个用户 ( '-r' 排除主目录) <br/>
usermod -c "User FTP" -g system -d /ftp/user1 -s /bin/nologin user1 修改用户属性 <br/>
passwd 修改口令 <br/>
passwd user1 修改一个用户的口令 (只允许root执行) <br/>
chage -E 2005-12-31 user1 设置用户口令的失效期限 <br/>
pwck 检查 '/etc/passwd' 的文件格式和语法修正以及存在的用户 <br/>
grpck 检查 '/etc/passwd' 的文件格式和语法修正以及存在的群组 <br/>
newgrp group_name 登陆进一个新的群组以改变新创建文件的预设群组 <br/>
<hr>

#### 打包和压缩文件
bunzip2 file1.bz2 解压一个叫做 'file1.bz2'的文件 <br/>
bzip2 file1 压缩一个叫做 'file1' 的文件 <br/>
gunzip file1.gz 解压一个叫做 'file1.gz'的文件 <br/>
gzip file1 压缩一个叫做 'file1'的文件 <br/>
gzip -9 file1 最大程度压缩 <br/>
rar a file1.rar test_file 创建一个叫做 'file1.rar' 的包 <br/>
rar a file1.rar file1 file2 dir1 同时压缩 'file1', 'file2' 以及目录 'dir1' <br/>
rar x file1.rar 解压rar包 <br/>
unrar x file1.rar 解压rar包 <br/>
tar -cvf archive.tar file1 创建一个非压缩的 tarball 
tar -cvf archive.tar file1 file2 dir1 创建一个包含了 'file1', 'file2' 以及 'dir1'的档案文件 <br/>
tar -tf archive.tar 显示一个包中的内容 <br/>
tar -xvf archive.tar 释放一个包 <br/>
tar -xvf archive.tar -C /tmp 将压缩包释放到 /tmp目录下 <br/>
tar -cvfj archive.tar.bz2 dir1 创建一个bzip2格式的压缩包 <br/>
tar -xvfj archive.tar.bz2 解压一个bzip2格式的压缩包 <br/>
tar -cvfz archive.tar.gz dir1 创建一个gzip格式的压缩包 <br/>
tar -xvfz archive.tar.gz 解压一个gzip格式的压缩包 <br/>
zip file1.zip file1 创建一个zip格式的压缩包 <br/>
zip -r file1.zip file1 file2 dir1 将几个文件和目录同时压缩成一个zip格式的压缩包 <br/>
unzip file1.zip 解压一个zip格式压缩包 <br/>
<hr>



