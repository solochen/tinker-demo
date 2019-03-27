# tinker-demo

### 这里的demo示例代码完全是拷贝官方的，这里只记录使用demo时踩的坑。

1. 导入demo运行时会一直报错，提示'tinkerId is not set!!!', 这是因为demo中tinerId使用的是git版本来作为唯一的tinkerId，这里我为了测试直接写死：
![img](https://github.com/solochen/tinker-demo/blob/master/app/image/d3.jpg)

2.修改完tinkerId后再次运行，Ok成功安装到手机上，此时在目录app/build/bakApk/中会生成两个文件：

![img](https://github.com/solochen/tinker-demo/blob/master/app/image/d1.jpg)

其中app-debug-0327-13-49-49.apk就是刚才安装在手机上的安装包

3. 我们将.apk的名字和.txt文件的名字复制下来进入build.gradle里面将tinkerOldApkpath和tinkerapplyResourcePath的路径替换成刚才你复制的，这样就等于指定了要等会热更新要替换的包
![img](https://github.com/solochen/tinker-demo/blob/master/app/image/d2.jpg)

4.以上是老的apk设置步骤，现在来看下怎么生成修复版补丁包以及补丁包怎么安装在老的版本中
5.在MainActivity中随便添加一个按钮，区别于老版本：
![img](https://github.com/solochen/tinker-demo/blob/master/app/image/d4_1.jpg)
6.双击运行gradle--Tasks--tinker--tinkerPatchDebug:
![img](https://github.com/solochen/tinker-demo/blob/master/app/image/d5.jpg)
运行完毕会在app/build/outputs/apk/tinkerPatch/debug/中生成patch_signed_7zip.apk文件，这个文件就是新apk与老apk的差异补丁包
![img](https://github.com/solochen/tinker-demo/blob/master/app/image/d6.jpg)

7.将这个补丁包拷贝到手机根目录，为什么是根目录呢，因为在MainActivity中Load patch的时候指定的补丁路径是在根目录：

![img](https://github.com/solochen/tinker-demo/blob/master/app/image/d7.jpg)

拷贝补丁包到根目录可通过adb命令：adb push ./app/build/outputs/apk/tinkerPatch/debug/patch_signed_7zip.apk /storage/sdcard0/

8.手机中打开demo，点击Load patch，重启demo，此时底部就出现了新增的按钮。
![img](https://github.com/solochen/tinker-demo/blob/master/app/image/d4_1.jpg)


# 注：检查存储权限是否开启
