在Ubuntu上SublimeText无法输入中文的解决方法
===

0. 下载sublime-imfix.c  
假设下载到了 home（～）目录下

0. 安装C\C++编译环境和gtk libgtk2.0-dev  
终端下输入以下命令：  
	`sudo apt-get install build-essential  libgtk2.0-dev`

0. 编译共享库  
终端下输入以下命令：  
	``gcc -shared -o libsublime-imfix.so sublime_imfix.c  `pkg-config --libs --cflags gtk+-2.0` -fPIC``

	> 该命令需要在 home 目录下执行， 即 **sublime-imfix.c** 所在目录  
	
	如果报错 `gcc: error: sublime_imfix.c: No such file or directory`，可以先进入系统设置>语言支持，查看是否有未安装完的包，如果有将自动安装，完成后再执行命令。

0. 将编译好的库移到 sublime 的安装目录  
终端下输入以下命令：  
	`mv libsublime-imfix.so $SUBLIME_HOME/`

	> 该命令需要在 home 目录下执行， 即 **libsublime-imfix.so** 所在目录  
	> **$SUBLIME_HOME**，指Sublime的安装（所在）目录

0. 启动 Sublime Text 3  
终端下输入以下命令：  
	`LD_PRELOAD=./libsublime-imfix.so ./sublime_text`

	> 该命令需要在 sublime 的安装目录下执行  
	> 否则，需要将命令中的两个文件换成绝对路径
	
0. 修改 .bashrc   
为了方便，可以在 **.bashrc** 中添加如下语句，这样在终端输入 *__subl__* 即可打开sublime并输入中文   
	`alias subl='LD_PRELOAD=/opt/sublime_text/libsublime-imfix.so /opt/sublime_text/sublime_text'`

	> ' '引号内需要输入绝对路径,如果安装位置不一样，请查看自己sublime安装位置并替换 

0. 从任务栏启动  
在ubuntu系统下，将sublime锁定到左侧任务栏，会有一个**sublime_text.desktop**，目录：  
`/usr/share/applications` （位置可能不同，自行`locate sublime_text.desktop`确认）

	修改该文件需要权限`sudo vim sublime_text.desktop`  

	将 **sublime_text.desktop** 文件中 *[Desktop Entry]* 下的 *Exec* 修改如下，然后<kbd>shfit</kbd>+<kbd>z</kbd>+<kbd>z</kbd>保存即可:  
	`Exec=bash -c 'LD_PRELOAD=/opt/sublime_text/libsublime-imfix.so /opt/sublime_text/sublime_text'`

	> ' '引号内需要输入绝对路径,如果安装位置不一样，请查看自己sublime安装位置并替换

#### 博客：http://my.oschina.net/zlLeaf/blog/185428
#### 来源：http://my.oschina.net/wugaoxing/blog/121281
#### 博客：http://www.cnblogs.com/memory4young/p/could-not-input-chinese-in-sublime-on-ubuntu.html
