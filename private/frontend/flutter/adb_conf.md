# Adb调试工具的使用
>adb调试工具地址一般在{CurrentUserDir}\AppData\Local\Android\Sdk\platform-tools\adb.exe
由于一些安全限制adb.exe进程总是被kill，所以只能手动启动。
1、修改程序名字adb.exe为adbrun.exe
2、添加环境变量Path=*;{CurrentUserDir}\AppData\Local\Android\Sdk\platform-tools\;
3、更换启动端口，新建环境变量ANDROID_ADB_SERVER_PORT=9999
4、设置手动启动调试工具（AS不启动，可选），新建环境变量 ANDROID_ADB_USER_MANAGED_MODE=true

#常用命令
1. 获取序列号：
```adbrun get-serialno```
2. 查看连接计算机的设备：
```adbrun devices```
3. 重启机器：
```adbrun reboot```
4. 重启到bootloader，即刷机模式：
```adbrun reboot bootloader```
5. 重启到recovery，即恢复模式：
```adbrun reboot recovery```
6. 查看log：
```adbrun logcat```
7. 终止adb服务进程：
```adbrun kill-server```
8. 重启adb服务进程：修改环境变量：
```adbrun -P 9999 start-server```
9. 获取机器MAC地址：
```adbrun shell  cat /sys/class/net/wlan0/address```
10. 获取CPU序列号：
```adbrun shell cat /proc/cpuinfo```
11. 安装APK：
```adbrun install <apkfile> //比如：adbrun install baidu.apk```
12. 保留数据和缓存文件，重新安装apk：
```adbrun install -r <apkfile> //比如：adbrun install -r baidu.apk```
13. 安装apk到sd卡：
```adbrun install -s <apkfile> // 比如：adbrun install -s baidu.apk```
14. 卸载APK：
```adbrun uninstall <package> //比如：adbrun uninstall com.baidu.search```
15. 卸载app但保留数据和缓存文件：
```adbrun uninstall -k <package> //比如：adbrun uninstall -k com.baidu.search```
16. 启动应用：
```adbrun shell am start -n <package_name>/.<activity_class_name>```
17. 查看设备cpu和内存占用情况：
```adbrun shell top```
18. 查看占用内存前6的app：
```adbrun shell top -m 6```
19. 刷新一次内存信息，然后返回：
```adbrun shell top -n 1```
20. 查询各进程内存使用情况：
```adbrun shell procrank```
21. 杀死一个进程：
```adbrun shell kill [pid]```
22. 查看进程列表：
```adbrun shell ps```
23. 查看指定进程状态：
```adbrun shell ps -x [PID]```
24. 查看后台services信息：
```adbrun shell service list```
25. 查看当前内存占用：
```adbrun shell cat /proc/meminfo```
26. 查看IO内存分区：
```adbrun shell cat /proc/iomem```
27. 将system分区重新挂载为可读写分区：
```adbrun remount```
28. 从本地复制文件到设备：
```adbrun push <local> <remote>```
29. 从设备复制文件到本地：
```adbrun pull <remote>  <local>```
30. 列出目录下的文件和文件夹，等同于dos中的dir命令：
```adbrun shell ls```
31. 进入文件夹，等同于dos中的cd 命令：
```adbrun shell cd <folder>```
32. 重命名文件：
```adbrun shell rename path/oldfilename path/newfilename```
33. 删除system/avi.apk：
```adbrun shell rm /system/avi.apk```
34. 删除文件夹及其下面所有文件：
```adbrun shell rm -r <folder>```
35. 移动文件：
```adbrun shell mv path/file newpath/file```
36. 设置文件权限：
```adbrun shell chmod 777 /system/fonts/DroidSansFallback.ttf```
37. 新建文件夹：
```adbrun shell mkdir path/foldelname```
38. 查看文件内容：
```adbrun shell cat <file>```
39. 查看wifi密码：
```adbrun shell cat /data/misc/wifi/*.conf```
40. 清除log缓存：
```adbrun logcat -c```
41. 查看bug报告：
```adbrun bugreport```
42. 获取设备名称：
```adbrun shell cat /system/build.prop```
43. 查看ADB帮助：
```adbrun help```
44. 跑monkey：
```adbrun shell monkey -v -p your.package.name 500```
