# linux下systemctl enable （service名）时出现file exists的解决办法

https://blog.csdn.net/m0_37876745/article/details/78188626



有时候我们在**Linux**下（我主要是是在centos平台，想来不同的平台应该差异没有那么大）配置好一项服务的时候，会使用一个命令，那就是



**systemctl enable （service名）**，



但是如果一次没有配置成功的话，经常需要重复的第二次配置，但是如果忘记remove时候，就会出现比较奇怪的错误，比如会提示报错说file exists，这时候就需要先**systemctl disable （服务名）。**

这是因为在systemctl enable （service名）时候，实际上是创建了一个链接，这里以我的vncserver@:1.service为例，我们可以看到实际上是创建了一个symlink，系统链接，从/etc/systemd/system/multi-user.target.wants/vncserver@:1.service 到 /etc/systemd/system/vncserver@:1.service![img](https://blog.csdn.net/m0_37876745/article/details/78188626)

我们这时候在/etc/systemd/system/下面也是能够看得到这个vncserver@:1.service的链接

![img](https://blog.csdn.net/m0_37876745/article/details/78188626)

所以再次操作的时候，要记得使用 systemctl disable 命令清除掉