sudo rmmod psmouse //禁用触摸屏
sudo modprobe -r psmouse //禁用触摸屏
sudo modprobe psmouse //启用触摸屏

#vim命令
:q   --退出 :quit的缩写
:q!  --不保存退出 :quit!的缩写
:wq  --写入文件并退出 :writequit的缩写
:wq! --如果文件只有读权限写入并退出，如果没有写权限，强制写
:x   --类似:wq
:qa  --退出全部 :quitall
ZZ   --如果文件有变动，写入/保存，然后退出
ZQ   --如果不想保存文件，也可以这个命令退出

#添加开机启动命令行
#ubuntu16.10开始不再使用initd管理系统，改用systemd
#systemd默认读取/etc/systemd/system下的配置文件，该目录下的文件会链接
#lib/systemd/system下的文件
#执行ls /lib/systemd/system，可以看到很多启动脚本，其中有我们需要的rc.local.service

#正常情况下启动文件主要分为三部分
#[Unit]段：启动顺序与依赖关系
#[Service]段：启动行为，如何启动，启动类型
#[Install]段：定义如何安装这个配置文件，即怎样做到开机启动

#添加如下：
[Install]
WantedBy=multi-user.target
Alias=rc-local.service

#ubuntu-18.04以后没有/etc/rc.local文件，需要自己创建
sudo touch /etc/rc.local

#对rc.local添加可执行的属性
sudo chmod +x /etc/rc.local

#对rc.local添加内容
#!/bin/sh -e
sudo modprobe -r psmouse //关闭触摸板
exit 0

#链接文件
sudo ln  -s /lib/systemd/system/rc.local.service /etc/systemd/system/






