# Linux 使用指南 #
## 1. 环境配置 ##
### 1.1Root 权限配置 ###
- root 用户是超级用户，拥有着 Linux 系统内最高的权限
- Ubuntu默认不设置root密码，直接使用sudo -i
- 使用visudo可编辑sudo配置
### 1.2 软件源配置 ###
```bash
# 更新软件源列表
sudo apt-get update
# 升级已安装软件包
sudo apt-get upgrade
```
## 2. 基本命令 ##
### 2.1 文件系统操作 ###
|命令|说明|常用选项|
|----|----|-------|
|cd [dir]|	切换目录|	`~`家目录 `/`根目录 `.`当前 `..`上级|
|pwd	 |显示当前目录
|ls|	列出目录内容	|`-l`详细信息 `-a`显示隐藏 `-h`易读大小 `-t`按时间排序 `-R`递归|
|mkdir	|创建目录	|-p创建多级目录|
|touch	|创建文件|
|cp	|复制文件	|`-r`递归复制目录 `-i`交互确认 `-v`显示进度|
|mv|	移动/重命名|	`-i`交互确认 `-f`强制覆盖|
|rm|	删除|	`-r`递归 `-f`强制|
- 查找指定进程的格式:
```
ps -ef | grep 进程关键字
```
- 进程的状态
```
ps [options] [--help]
--help 查看帮助
-A 列出所有的进程
-au 显示较详细的资讯
-aux 显示所有包含其他使用者的进程
```
![alt text]()
### 2.2 解压缩 ###
|格式|	解压命令	|压缩命令|
|----|-------------|-------|
|.tar|	tar -xvf file.tar|	tar -cvf file.tar dir|
|.gz|	gunzip file.gz|	gzip file|
|.tar.gz|	tar -zxvf file.tar.gz|	tar -zcvf file.tar.gz dir|
|.zip|	unzip file.zip|	zip file.zip dir|
|.rar|	rar -x file.rar|	rar -a file.rar dir
|.bz2|解压1：bzip2 -d FileName.bz2 解压2：bunzip2 FileName.bz2|bzip2 -z FileName|
### 2.4 其他常用命令 ###
```bash
sudo command ## 使用root权限执行
time command
OMMAND < file #表示将指定文件作为执行命令中的输入
COMMAND > file #表示将命令输出至指定文件而非终端
```
## 3. 软件安装 ##
### 3.1 APT包管理 ###
```bash
sudo apt update    # 列出所有可更新的软件清单
sudo apt upgrade    # 升级软件包
sudo apt install <package_name>    # 安装指定的软件包
sudo apt remove <package_name>    # 删除指定软件包
sudo apt autoremove    #清理不再使用的依赖和库文件
apt list --upgradeable    #列出可更新的软件包及版本信息
```
### 3.2 DEB包安装(.deb 文件) ###
```bash
dpkg --help 显示帮助
sudo dpkg -i file.deb 安装指定的软件包
```
### 软件带有安装程序 ###
```bash
# 添加执行权限
chmod +x install.sh
# .sh文件,.pl 文件
sudo ./install.sh
```
## 4. 文本编辑 - Vim ##
### 4.1 三种模式 ###
*类 Unix 系统中都内建有 vi vim*
*除了直接按 vi/vim 以外还可以通过后接文件名的方式来直接打开指定文件， 如 `sudo vi /etc/apt/sources.list`*
- 命令模式：启动时的默认模式  
```
i  -- 切换到输入模式，在光标当前位置开始输入文本。
x  -- 删除当前光标所在处的字符。
:  -- 切换到底线命令模式，以在最底一行输入命令。
dd -- 剪切当前行。
yy -- 复制当前行。
p  -- 粘贴剪贴板内容到光标下方。
P  -- 粘贴剪贴板内容到光标上方。
u  -- 撤销上一次操作。
Ctrl+r -- 重做上一次撤销的操作。
```
- 输入模式：按i进入，编辑文本
    - 在命令模式按`i`进入，按 `Esc` 则返回普通模式
- 底线命令模式：按`:`进入
```bash
:w  -- 保存文件。
:q  -- 退出 Vim 编辑器。
:wq -- 保存修改并退出 Vim 编辑器。
:q! -- 不保存修改强制退出 Vim 编辑器。
```
