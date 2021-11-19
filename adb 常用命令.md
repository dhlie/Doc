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

### 打发布包:
    1. 梆梆加固
        release 包用梆梆客户端加固
        导出未签名包
        签名:apksigner sign --ks D:\dev\fddAssistant --out C:\Users\duanhaoliang\Desktop\signed.apk --in a.apk
    2. 乐固加固
        release 在线加固:https://console.cloud.tencent.com/ms/index
        下载加固包
        对齐(否则在Android11安装失败):zipalign -p -f -v 4 input.apk output_unsigned.apk
        签名:apksigner sign --ks D:\dev\fddAssistant --out C:\Users\duanhaoliang\Desktop\signed.apk --in a.apk