转自：https://blog.csdn.net/lidew521/article/details/85268374

### 一、Linux中的一些病毒,经常会修改,文件的权限为特殊权限,就连root用户也动不了这个文件,所以这个命令需要记一下

Linux系统中，拥有最高权限的用户root，在执行文件权限的修改，或者修改文件时也会出现如下错误：

1. chmod: changing permissions of 'xxx': Operation not permitted；

2. E45: 'readonly' option is set (add ! to override)

接下来本文主要介绍如何解决root用户无权限修改文件的问题。

一般我们都知道，Linux下root用户的权限是最大了，难道还有root用户操作不了的文件？

(Linux下UID数值越小的用户，权限越大，可以看到最小值为0，即root用户）



```ruby
END
```

### 二、问题修复

1. 上面我们执行的chmod命令，其底层实现是chattr命令，用此命的功能更为强大，甚至可以锁定文件，即使root用户也操作不了此文件。

1. chattr是用来更改文件属性，lsattr可用来查看文件的属性，执行命令lsattr /etc/sysctl.conff便可以看到当前文件的属性；

   可以发现当前文件有个i属性，查阅命令帮助文档可以看到有i属性的文件是不能修改的，更不可被删除，即使是root用户也不可。

1. 3

   既然知道了文件不能操作的原因是加了i属性，所以相应的解决方案就是把文件的i属性去除，然后对此文件内容进行修改，最好在操作完成后恢复文件的i属性。

   **去除i属性：**chattr -i /etc/sysctl.conf

   **添加i属性：**chattr +i /etc/sysctl.conf

chattr：change attributes

chmod：change mode