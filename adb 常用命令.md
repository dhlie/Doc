### 更新相册

    adb shell am broadcast -a android.intent.action.MEDIA_SCANNER_SCAN_FILE -d file:///sdcard/down.png

### apk 签名

    apksigner sign --ks D:\dev\fddAssistant --out C:\Users\duanhaoliang\Desktop\signed.apk --in a.apk
    apksigner verify -v --print-certs a.apk

### kill 进程

    adb shell am force-stop package-name
    adb shell am kill package-name  //杀死后台应用

### 查看进程

    adb shell ps | grep "d.hl"

### 查看栈顶 Activity

    adb shell dumpsys activity activities | grep ResumedActivity

### 清除应用数据
    adb shell pm clear pname

### 打发布包:

    1. 乐固加固
        a. release 包在线加固:https://console.cloud.tencent.com/ms/index
        b. 下载加固包
        c. 对齐(否则在Android11安装失败):zipalign -p -f -v 4 input.apk output_unsigned.apk
        d. 签名:apksigner sign --ks D:\dev\fddAssistant --out C:\Users\duanhaoliang\Desktop\signed.apk --in a.apk

### crash 关键字搜索
    Fatal > Crash > AndroidRuntime > Exception>Error