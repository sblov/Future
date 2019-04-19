## Ubuntu-18.04-Configuration

### Unity

```shell
#切换unity
sudo apt-get install unity
sudo apt-get install unity-tweak-tool

#Numix 主题
sudo add-apt-repository ppa:numix/ppa
sudo apt-get update
sudo apt-get install numix-gtk-theme numix-icon-theme-circle  
```

### Java

```shell
#创建JDK目录
sudo mkdir /usr/lib/jvm 
#解压下载的jdk包到这个目录下
 sudo tar -zxvf jdk-.....gz  -C  /usr/lib/jvm 
 #创建及加入java环境变量
 sudo vim ~/.bashrc  
```

```shell
JAVA_HOME=/usr/lib/jvm/jdk1.8.0_121 
JRE_HOME=$JAVA_HOME/jre 
JAVA_BIN=$JAVA_HOME/bin CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin 
export JAVA_HOME JRE_HOME PATH CLASSPATH
```

```shell
#update
source ~/.bashrc
#TEST
java -version
```

### NodeJs

```shell
tar xvJf  node-...-linux-x64.tar.xz
ln -s /opt/node-...-linux-x64/bin/node /usr/local/bin/node
ln -s /opt/node--...-linux-x64/bin/npm /usr/local/bin/npm
ln -s /opt/node-...-inux-x64/bin/npx /usr/local/bin/npx
#TEST
node -v
```

### Tomcat

​	直接解压

### SogouPinyin

`https://pinyin.sogou.com/linux/`

​	***config..................***

```shell
#防止乱码
cd ~/.config
rm -rf SogouPY* sogou*
#卸载fctix的输入面板
sudo apt-get remove fcitx-qimpanel
reboot
```

### Bash_launcher_icon

```shell
#Backup
sudo mv /usr/share/unity/icons/launcher_bfb.png /usr/share/unity/icons/launcher_bfb.png.bak
#Copy
sudo cp launcher_bfb.png  /usr/share/unity/icons/
```

### Guitar

`https://github.com/soramimi/Guitar`

`http://files.soramimi.jp/guitar/`

### Git

```shell
sudo add-apt-repository ppa:git-core/ppa 
sudo apt-get update 
sudo apt-get install git
#TEST
git --version      
```

### Typora

`https://www.typora.io/#linux`

```shell
wget -qO - https://typora.io/linux/public-key.asc | sudo apt-key add -
sudo add-apt-repository 'deb https://typora.io/linux ./'
sudo apt-get update
sudo apt-get install typora
```

### Sublime

`http://www.sublimetext.com/3`

​	config...................

```shell
git clone https://github.com/lyfeyaj/sublime-text-imfix.git
cd sublime-text-imfix && ./sublime-imfix
#注意配置问题，.desktop文件中对应路径，sublime-imfix内配置路径
```

### HTML5

```shell
sudo apt-get install ubuntu-restricted-extras
#reboot firefox
```

### Lantern

`https://github.com/getlantern/lantern`

### Ubuntu-desktop

#### change

```shell
gedit user.po
```

```shell
msgid "Ubuntu Desktop"
msgstr "lov" 
```

```shell
gedit startShell.sh
```

```shell
# 获取当前路径
Cur_Dir=$(pwd)
echo pwd：$Cur_Dir

echo TITLE_LOCAL
#cd /usr/share/locale/en/LC_MESSAGES        # 英文环境
cd /usr/share/locale/zh_CN/LC_MESSAGES      # 中文环境

echo	SET_TITLE
sudo msgfmt -o unity.mo $Cur_Dir/user.po

echo SUCCESS
```

```shell
sh startShell.sh
```

#### reinstall

```shell
sudo apt-get install --reinstall ubuntu-desktop  
sudo apt-get install unity  
reboot
```

1. 查看当前系统安装了几个桌面

ls /usr/share/xsessions


2. 修改配置文件

sudo vi /usr/share/lightdm/lightdm.conf.d/50-ubuntu.conf

[SeatDefaults]
user-session=ubuntu

将 Ubuntu 修改为 你的桌面即可

### 触屏单击

```shell
sudo apt install xserver-xorg-input-libinput
sudo vim /usr/share/X11/xorg.conf.d/...-libinput.conf (以libinput.conf结尾文件)
```

```shell
.........
Section "InputClass"
        Identifier "libinput touchpad catchall"
        Option "Tapping" "on"	#启用触摸板点击
        Option "ClickMethod" "clickfinger"	#单指左键双指右键，三指中键
        #Option "NaturalScrolling" "true"	#启用自然滚动
        #Option "DisableWhileTyping" "True"	#打字的时候禁用触摸板
        MatchIsTouchpad "on"
        MatchDevicePath "/dev/input/event*"
        Driver "libinput"
EndSection
........
```

重启

## Mysql 

`https://dev.mysql.com/doc/refman/8.0/en/linux-installation-debian.html`：文档

1、下载DEB Bundle的tar包（ubuntu mysql下）

`tar xvf mysql-server_xxx-1ubuntuxxx_amd64.deb-bundle.tar `

2、根据文档提示安装，最好一条条执行deb包，不批量执行（`sudo apt-get -f install`安装未安装依赖）

> 数据库目录：`/var/lib/mysql/ `
>
> 配置文件：`/usr/share/mysql（`命令及配置文件） ，`/etc/mysql（`如：my.cnf）
>
> 相关命令：`/usr/bin`(mysqladmin mysqldump等命令) 和`/usr/sbin`
>
> 启动脚本：`/etc/init.d/mysql`（启动脚本文件mysql的目录）

> 服务启动后端口查询
> `sudo netstat -anp | grep mysql`
> 启动
> `sudo service mysql start`
> 停止
> `sudo service mysql stop`
> 服务状态
> `sudo service mysql status`

## NVIDIA显卡驱动

​	由于ubuntu系统对显卡支持问题，会因为没有相应的显卡驱动加大cpu的负荷，应安装对应显卡的驱动

**驱动下载：** https://www.geforce.cn/drivers

​	下载 `NVIDIA-Linux-x86_64-418.56.run`（对应驱动文件）

**安装**

1、ubuntu 默认安装了第三方开源的驱动程序nouveau，安装nvidia显卡驱动首先需要禁用nouveau，不然会碰到冲突的问题，导致无法安装nvidia显卡驱动

> `sudo vim /etc/modprobe.d/blacklist.conf `
>
> 添加：
>
> ```shell
> blacklist nouveau
> 
> options nouveau modeset=0
> ```

2、更新系统并重启

> `sudo update-initramfs -u`
>
> `reboot`

3、验证nouveau是否已禁用

> `lsmod | grep nouveau`    #无返回值则禁用成功

4、切入命令行执行（`ctrl+alt+f1`）

> ```shell
> sudo service lightdm stop      #这个是关闭图形界面，不执行会出错
> 
> sudo apt-get remove nvidia-*  #卸载掉原有驱动（若安装过其他版本或其他方式安装过驱动执行此项）
> 
> sudo chmod  a+x NVIDIA-Linux-x86_64-418.56.run    #驱动run文件赋予执行权限`
> 
> sudo ./NVIDIA-Linux-x86_64-396.18.run -no-x-check -no-nouveau-check -no-opengl-files #只有禁用opengl这样安装才不会出现循环登陆的问题
> ```
>
> *-no-x-check：安装驱动时关闭X服务*
>
> *-no-nouveau-check：安装驱动时禁用nouveau*
>
> *-no-opengl-files：只安装驱动文件，不安装OpenGL文件*

5、挂载nvidia驱动

> `modprobe nvidia`
>
> `nvidia-smi	#查看nvidia信息，检查驱动是否安装成功`  
>
> `reboot`

