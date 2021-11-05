更新相册
adb shell am broadcast -a android.intent.action.MEDIA_SCANNER_SCAN_FILE -d file:///sdcard/down.png

apk 签名
apksigner sign --ks D:\dev\fddAssistant --out C:\Users\duanhaoliang\Desktop\signed.apk --in a.apk
apksigner verify -v --print-certs a.apk

kill 进程
adb shell am force-stop package-name
adb shell am kill package-name  //杀死后台应用