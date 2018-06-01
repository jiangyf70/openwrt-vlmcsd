openwrt-vlmcsd
-----
#### An OpenWRT package for vlmcsd.

You can use [luci-app-vlmcsd](https://github.com/cokebar/luci-app-vlmcsd "") to control it. luci-app-vlmscd support KMS auto-activation.

Travis CI: [![Build Status](https://travis-ci.org/cokebar/openwrt-vlmcsd.svg?branch=master)](https://travis-ci.org/cokebar/openwrt-vlmcsd)

Using without luci-app-vlmcsd
-----
If you don't use luci-app-vlmcsd and you want vlmcsd support KMS auto activation, you should modify the settings of dnsmasq manually:

1. Add the following line at the end of `/etc/dnsmasq.conf`:

   `srv-host=_vlmcs._tcp.lan,hostname.lan,1688,0,100`
   
   (replace "hostname.lan" with your actual host name, eg: openwrt.lan, or just replace it with your IP of LAN）

2. Restart dnsmasq:

   `/etc/init.d/dnsmasq restart`

   You can check if the dnsmasq setting works with the following cammand in Windows:
   
   `nslookup -type=srv _vlmcs._tcp.lan`
   
   The response should be your router's IP.

3. `/etc/init.d/vlmcsd enable && /etc/init.d/vlmcsd start && /etc/init.d/dnsmasq restart`

Pre-compiled Download
-----
Your can find pre-compiled ipk:
https://github.com/cokebar/openwrt-vlmcsd/tree/gh-pages
编译方法：
——————————————————————————————————————————————————
cd package/

git clone https://github.com/cokebar/luci-app-vlmcsd.git

git clone https://github.com/cokebar/openwrt-vlmcsd.git

cd ..

make menuconfig


/ 搜索vlmcsd位置

make package/openwrt-vlmcsd/compile V=s

make package/luci-app-vlmcsd/compile V=s


——————————————————————————————————————————————————————————————

将要编译的软件包代码 clone 到 package 目录

git clone https://github.com/ysc3839/openwrt-vlmcsd.git package/vlmcsd

执行 make menuconfig 并选中你要编译的软件。我这里编译时是自动选中的，所以直接保存即可。

执行 make package/vlmcsd/compile V=s 开始编译。

V=s 参数代表输出所有编译信息，可以不加。但如果编译出错，要加上此参数才能看到错误信息。

加上 -jN 指令可以让 N 个编译任务同时运行。这里的 N 建议设为 CPU 核心数+1，比如我的 CPU 是 8 核心，参数就是 -j9。

对于本文的例子，因为只编译一个软件包，加上此参数并不能带来明显的速度提升，所以我说不加的。

编译后生成的 ipkg 可以在 bin 目录中找到。
附加说明

文中的 openwrt-vlmcsd 项目 fork 自 cokebar/openwrt-vlmcsd。我修正了 bug 并做了部分改进。

cokebar 还为 vlmcsd 编写了 luci 界面 luci-app-vlmcsd，可以参考本文的方法来编译。

KMS 有自动发现内网中激活服务器的功能。如果你安装了 luci-app-vlmcsd，选择 Auto activate 或 自动激活 选项并应用即可。

如果你没安装 luci-app-vlmcsd，可以手动编辑 /etc/dnsmasq.conf，在文件最后面加入:

srv-host=_vlmcs._tcp.lan,hostname.lan,1688,0,100

保存后重启 dnsmasq: /etc/init.d/dnsmasq restart

在 Windows 中执行 nslookup -type=srv _vlmcs._tcp.lan 可以检测是否设置成功。
