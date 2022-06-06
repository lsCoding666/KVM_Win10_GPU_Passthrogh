# Manjaro Kvm 显卡直通

我的配置

神舟z8 ta5ns

cpu 11260h

gpu 3060 laptop

非独显直连 传统的核显输出独显渲染。有一个独显输出的minidp口。



弄了一天 主要参考了

Chinese:

https://lantian.pub/article/modify-computer/laptop-muxed-nvidia-passthrough.lantian/

English:

https://lantian.pub/en/article/modify-computer/laptop-muxed-nvidia-passthrough.lantian/

这篇文章 感谢这位作者



说几个注意的地方

```xml
  <cpu mode="host-passthrough" check="none" migratable="on">
    <topology sockets="1" dies="1" cores="6" threads="2"/>
      下面这一行过检测
    <feature policy="disable" name="hypervisor"/>
  </cpu>
```

系统一定要win10 pro 不要用ltsc 2019 2021。我用2021一直有问题。

显卡直通完成后，我的做法是拿淘宝买的一个虚拟显示器（显卡欺骗器），插在笔记本电脑的minidp口转hdmi线上，然后将原本的虚拟显卡删掉（改为none）。

装好后先用krdc 远程到win10上（通过RDP），然后安装Parsec，并设置为开机自启。这样以后每次启动机器后宿主机的Parsec就能直接连接到虚拟机上。Parsec远程连接到虚拟机后，打开gforce experience，点击设置，点击shield，这样moonlight才可以用。当然，如果你直接选择使用Parsec而不是moonlight也是可以的。

如果你手上现在只有一部笔记本电脑，没有minidp转hdmi线和显卡欺骗器的话，只能使用kvm一开始的那个虚拟显卡来显示内容了。你可以将虚拟显卡从QXL改为Virtio，并开启3d加速，这样可以调整分辨率到1080P，然后通过Parsec连接会报错，但是可以使用moonlight连接。连接上后你会发现无论在moonlight还是在虚拟机管理器上的显示都会很卡，这是因为独显的显示最终通过虚拟显卡输出的，原本60帧的游戏通过这种方式只能20-30帧。而且键鼠操作有明显的延迟。



所以最好的方案还是独显直连方案，买一个显卡欺骗器，插在独显输出的minidp上或者是任何一个是独显输出的口上，然后删除掉原本的虚拟显示器，然后再用Parsec或者说moonlight远程过去，这样画面才不会卡。



如果宿主机连接虚拟机时提示没有硬件加速之类的提示，安装intel-media-driver。arch系列的安装命令是

```
yay -S intel-media-driver
```

安装好后重启 就不会弹出有关提示了

如果你一直解决不了这个问题，可以用软解，也可以玩。但是moonlight的启动会很慢，建议用Parsec

待更新。。。。