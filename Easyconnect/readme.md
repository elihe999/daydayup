下载easyconnect客户端

wget http://download.sangfor.com.cn/download/product/sslvpn/pkg/linux_767/EasyConnect_x64_7_6_7_3.deb

安装easyconnect

$ sudo dpkg -i EasyConnect_x64.deb
打开easyconnect，Harfbuzz version too old

$ /usr/share/sangfor/EasyConnect/EasyConnect 
(EasyConnect:71583): Pango-ERROR **: 15:11:47.910: Harfbuzz version too old (1.3.1)
有网友发现可以通过降级pango等依赖解决问题。错误信息提示Harfbuzz版本太旧了，实际上是因为pango版本太新了。需要做的不是升级Harfbuzz,而是降级pango。为了防止修改系统库带来的风险，直接将相关的so库文件解压到easyconnect同目录下即可。具体来说，涉及的so文件为：

 $ ldd EasyConnect | grep pango
	libpangocairo-1.0.so.0 => /lib/x86_64-linux-gnu/libpangocairo-1.0.so.0 (0x00007f6287eca000)
	libpango-1.0.so.0 => /lib/x86_64-linux-gnu/libpango-1.0.so.0 (0x00007f6287d30000)
	libpangoft2-1.0.so.0 => /lib/x86_64-linux-gnu/libpangoft2-1.0.so.0 (0x00007f628608a000)


先上传到/tmp再移到/usr/share/sangfor/EasyConnect/目录下

$ sudo mv /tmp/libpango* /usr/share/sangfor/EasyConnect/EasyConnect 
提示出错：Failed to load module "canberra-gtk-module"

Gtk-Message: 16:34:56.270: Failed to load module "canberra-gtk-module"
只要安装下

sudo apt-get install libcanberra-gtk-module -y
启动出现闪退：

解决方法：在登录进度条大概70%左右执行命令

  $sudo /bin/bash /usr/share/sangfor/EasyConnect/resources/shell/sslservice.sh
可以解决登录几秒后闪退问题

可以写一个脚本easyconnect放在桌面（要在终端执行输入），前提是easyconnect要保存密码和自动登录 

#!/bin/bash
sudo /usr/share/sangfor/EasyConnect/EasyConnect >  /dev/null 2>&1 &
sudo /bin/bash /usr/share/sangfor/EasyConnect/resources/shell/sslservice.sh
————————————————
版权声明：本文为CSDN博主「运维自动化&amp;云计算」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/h106140873/article/details/114263954