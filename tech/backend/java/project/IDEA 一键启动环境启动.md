由于我没有云服务器，所以利用中秋假期配置了 IDEA 一键快速启动开发所需要的环境(windows)：
1. 本地 Redis server
2. 本地 Linux
3. 本地 MySQL

IDEA 提供了一系列直接启动脚本配置，所以非常方便。

Redis 服务：
```bat
D:/environment/redis/redis-server.exe
```

本地 Linux，我所用的虚拟机软件是 VM VirturalBox，但是每次关闭电脑后都需要启动这个软件然后运行虚拟机，有点麻烦。不过，它提供了命令的方式启动虚拟机(后台运行),这对我来说非常方便:
* 后台运行虚拟机
``` bat
E:\software\tool\virtualbox\VBoxManage.exe startvm 虚拟机名称或ID -type headless
```

* 保持状态关闭虚拟机(推荐)
```bat
E:\software\tool\virtualbox\VBoxManage.exe controlvm 虚拟机名称或ID savestate
```
* 放弃已保存的状态
```bat
E:\software\tool\virtualbox\VBoxManage.exe discardstate 虚拟机名称或ID
```

* 断电关闭虚拟机
```bat
E:\software\tool\virtualbox\VBoxManage.exe controlvm 虚拟机名称或ID poweroff
```

* 正常关机(不能彻底关闭，一直处于stopping状态)
```bat
E:\software\tool\virtualbox\VBoxManage.exe controlvm  虚拟机名称或ID acpipowerbutton
```
> 当然，也可以配置环境变量，这样启动更加方便。


至此，所有环境都能通过 IDEA 一个软件启动。

---
参考:
1. [VBoxManage命令之虚机开启与关机_purple.taro的博客-CSDN博客_vboxmanage 关闭虚拟机](https://blog.csdn.net/zxlyx/article/details/118850727)