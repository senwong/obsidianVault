
# su和sudo区别
我们在以管理员执行命令时，经常使用`sudo [command] [args]`, 有时候可以使用`su root`切换到`root`用户，这两种方式有什么区别呢？
__执行原理__：`su`全称`switch user`, 执行`su root`时会切换到`root`用户，而`sudo [command]`在不会切换用户，在执行命令时是以root执行，但是执行完成后会退出到普通给用户。
__优缺点__：`su`的优点是可以完全拥有root权限，缺点是不安全，可能由于用户的误操作引起系统的。`sudo`的优点是可以配置
__使用经验__：能使用sudo就不使用su。

有时候会提示`sudo command not found`:
sudo下的PATH和普通用户的PATH不一样，sudo下使用的secure_path，配置在`/etc/sudoers`文件里，可以使用`sudo visudo`编辑配置文件。


# 磁盘
磁盘挂载，`mount`命令，linux系统中的设备都是以文件格式存在的 `/dev/disk`。
