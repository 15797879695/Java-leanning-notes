# Monkey的常见问题和解决办法

1.编译app里面的源码时，有时候由于模拟器闲置太久，把apk文件添加到模拟器中会报错：error:device not found

　　通常这个时候，我只需要:

　　adb kill-server

　　adb remount

　　然后就可以把我的apk安装到模拟器里。

　　2.今天，再试这种办法突然不行了，老是出现"error:device offline"，google了下，发现只需要添加一行命令就可以了，具体如下：







　　然后把我的apk安装到模拟器成功。

　　3 [**Android**](http://www.blogjava.net/qileilove/archive/2013/08/23/403216.html)中ADB [**server**](http://www.blogjava.net/qileilove/archive/2013/08/23/403216.html) didn't ACK fail to start daemon,怎么解决呢？

　　ADB server didn't ACK

　　在使用adb命令，有时候会出现下面这种错误。







　　因为有另外一个应用占用了ADB SERVER的端口，也许是adb [**shell**](http://www.blogjava.net/qileilove/archive/2013/08/23/403216.html)本身，结束进程中的adb.exe并重新启动adb试试。。

　　4、D:\android\android-sdk-windows\tools目录中没有adb.exe

　　检查一下你的android-sdk-windows目录下有没有platform-tools目录。并检查访目录下是否有adb.exe

　　5、在cmd中执行“adb shell”，则提示“error: device not found”

　　没有发现设备。如果用[**手机**](http://www.blogjava.net/qileilove/archive/2013/08/23/403216.html)设备[**测试**](http://www.blogjava.net/qileilove/archive/2013/08/23/403216.html)，请用USB数据线与电脑连接，同时并安装好手机驱动；如果你采用模拟器测试，你应该先启动eclipse，然后设置好模拟器，并进入模拟器测试状态

　　6、执行“adb shell”，提示“error: more than one device and emulator”

　　第一种情况：确实用多个设备或者模拟器

　　错误说明是有一个以上的设备和仿真器。这是因为我启用了模拟器，同时我也将我的手机通过USB线连接到了电脑。所以这里存在两个设备。这种情况下，需要指定连接某一个设备或者模拟器。





1、获取模拟器或设备列表

　　命令：adb devices

　　来查看存在几个设备

　　2、指定device来执行adb shell

　　命令：adb -s devicename shell

　　在多个device的时候，执行adb命令一般都需要用参数-s指定device。

　　例如卸载emulator-5554上的包com.soft.camera，命令如下

　　adb -s emulator-5554 uninstall com.soft.camera

　　例如通过monkey测试

　　adb -s emulator-5554 shell monkey -v -p your.www.com 500

　　**第二种情况：确实有一个设备或者模拟器**

　　之所以显示有多个，是因为此设备用过两次。使用命令adb devices会看到这个设备有两种状：offline和device，有以下两种解决办法：

　　1、重启adb.exe服务

　　一般办法：在命令窗口输入 adb start-server 重新启动adb.exe服务

　　2、终极方法，结束adb进程

　　如果上述方法不可以，终极方法：打开任务栏，选择“进程”选项卡，找到adb.exe进程，结束之，重新启动就完事OK啦（或者在命令窗口输入命令 adb kill-server ）

　　**7、在手机上通过monkey测试**

　　在cmd中输入：adb -s HT21JV204550 shell monkey -v -p your.www.com 500

　　出现：No activities found to run, monkey aborted.

　　原因：起初我是通过模拟环境测试，通过手机测试，应该将“Target”选项卡中由原来的“Automatic”项改为“Manual”

　　然后选择“Target”选项卡的前面一个选项卡“Android”，点击下面的“Run”。执行Run后结果如下：

　　点击OK，程序会将APK写入手机，这时就可以在cmd中输入命令：adb -s HT21JV204550 shell monkey -v -p your.www.com 500 ，通过在真实手机环境下进行测试了。

　　**8、关于monkey命令简要概述**

　　**一、九个事件及百分比控制**

　　**1、九个事件**

　　--pct-touch <percent> 0

　　调整触摸事件的百分比(触摸事件是一个down-up事件，它发生在屏幕上的某单一位置)（——点击事件，涉及down、up）

　　--pct-motion <percent> 1

　　调整动作事件的百分比(动作事件由屏幕上某处的一个down事件、一系列的伪随机事件和一个up事件组成)（——注：move事件，涉及down、up、move三个事件）

　　--pct-trackball <percent> 2

　　调整轨迹事件的百分比(轨迹事件由一个或几个随机的移动组成，有时还伴随有点击)--（轨迹球）

　　--pct-nav <percent> 3

　　调整“基本”导航事件的百分比(导航事件由来自方向输入设备的up/down/left/right组成)

　　--pct-majornav <percent> 4

　　调整“主要”导航事件的百分比(这些导航事件通常引发图形界面中的动作，如：5-way键盘的中间按键、回退按键、菜单按键)

　　--pct-syskeys <percent> 5

　　调整“系统”按键事件的百分比(这些按键通常被保留，由系统使用，如Home、Back、Start Call、End Call及音量控制键)

　　--pct-appswitch <percent> 6

　　调整启动Activity的百分比。在随机间隔里，Monkey将执行一个startActivity()调用，作为最大程度覆盖包中全部Activity的一种方法。（从一个Activity跳转到另一个）