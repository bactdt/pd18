# 汉化. 
搬运于https://github.com/bactdt/ParallelsDesktopCrack
原作者库已消失

只适合Parallels Desktop 18.0.1-53056

- [x] 支持英特尔
- [x] Apple支持 (M1 和 M2)
- [x] 网络
- [x] USB

# 自动安装

1. 安装 Parallels Desktop 18.0.1-53056.

    https://download.parallels.com/desktop/v18/18.0.1-53056/ParallelsDesktop-18.0.1-53056.dmg

2. 登陆或注册parallels account.

3. 下载此分支

4. 将此目录拖进终端。

5. `chmod +x ./install.sh && sudo ./install.sh`
# 手动安装法
1. 打开终端
```
cd ~/Downloads/ParallelsDesktop_18.0.1.53056_Patch
```

2. 退出Parallels Desktop

```
killall -9 prl_client_app
killall -9 prl_disp_service
```

2. 复制破解文件

```
sudo cp -f prl_disp_service "/Applications/Parallels Desktop.app/Contents/MacOS/Parallels Service.app/Contents/MacOS/prl_disp_service"
sudo chown root:wheel "/Applications/Parallels Desktop.app/Contents/MacOS/Parallels Service.app/Contents/MacOS/prl_disp_service"
sudo chmod 755 "/Applications/Parallels Desktop.app/Contents/MacOS/Parallels Service.app/Contents/MacOS/prl_disp_service"
```

3. 复制license.json

```
sudo rm -f "/Library/Preferences/Parallels/licenses.json"
sudo cp licenses.json "/Library/Preferences/Parallels/licenses.json"
sudo chown root:wheel "/Library/Preferences/Parallels/licenses.json"
sudo chmod 444 "/Library/Preferences/Parallels/licenses.json"
```

4. 结束

```
sudo codesign -f -s - --timestamp=none --all-architectures --deep --entitlements ParallelsService.entitlements "/Applications/Parallels Desktop.app/Contents/MacOS/Parallels Service.app/Contents/MacOS/prl_disp_service"
```

# 注意

Parallels Desktop可能会将客户端信息或日志上传到服务器。

您可以在使用防火墙阻止。

或使用域名, AdGuardHome 过滤器 DNS resolve.

```
download.parallels.com
update.parallels.com
desktop.parallels.com
download.parallels.com.cdn.cloudflare.net
update.parallels.com.cdn.cloudflare.net
desktop.parallels.com.cdn.cloudflare.net
www.parallels.cn
www.parallels.com
reportus.parallels.com
parallels.com
parallels.cn
pax-manager.myparallels.com
myparallels.com
my.parallels.com
```
#感谢
感谢大佬[@somebasj](https://github.com/somebasj)的维护
