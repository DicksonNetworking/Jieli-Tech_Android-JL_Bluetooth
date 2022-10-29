# Android-JL_OTA
The bluetooth OTA for Android

## quick start

In order to help developers quickly access the Jili OTA solution, please read the SDK development documents in detail before development: [Jili OTA External Library Development Document (Android)](https://doc.zh-jieli.com/Apps/Android/ ota/en-us/master/index.html).


## Access Q&A

Answers to common problems reported by developers in a unified way. When developers encounter problems, they can refer to [Access Q&A](https://gitee.com/Jieli-Tech/Android-JL_OTA/wikis/%E6%9D% B0%E7%90%86OTA%E5%BA%93%20%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98%E7%AD%94%E7%96% 91). <br/>
If you still can't solve the problem, please submit an issue and we will reply as soon as possible.

## Compressed package file structure description
  apk -- test APK<br>
  code -- demo program source code<br>
  doc -- development documentation<br>
  libs -- core libraries<br>


## Instructions for use
1. Open the app (the first time you open the app, you need to grant the corresponding permission)
2. Copy the upgrade file to the fixed storage location of the phone<br>
  * If the phone system is Android 10.0+, put it in /Android/data/com.jieli.otasdk/files/com.jieli.otasdk/upgrade/<br>
  * If the mobile phone system is below Android 10.0, put it in /com.jieli.otasdk/upgrade/
3. Connect the upgrade target device
4. Select the target upgrade file to start the OTA upgrade

## Upgrade method description
2. Customers can choose SDK development based on jl_bt_ota, refer to com.jieli.otasdk/tool/ota/.


| Library Name | Advantages | Disadvantages | Remarks |
| --- | --- | --- | --- |
| jl_bt_ota | 1. The OTA process is solidified, and it does not participate in the connection process, which is convenient for customers to change<br> 2. It does not affect the customer's reason agreement, and some functions can be accessed | 1. The customer needs to implement interfaces such as connection process and data transparent transmission<br> 2. Access is relatively complicated | Recommended |


**Device communication method:** Default is <strong style="color:#00008D">BLE</strong>, optional <strong style="color:#00008D">SPP</strong>, requires ** firmware **support.

## OTA upgrade parameter description

**OTAManager**
````java
   val bluetoothOption = BluetoothOTAConfigure()
   // select the communication method
   bluetoothOption.priority = BluetoothOTAConfigure.PREFER_BLE
   //Do you need to customize the back connection method (not required by default, if you need to customize the back connection method, you need to implement it yourself)
   bluetoothOption.isUseReconnect = !JL_Constant.NEED_CUSTOM_RECONNECT_WAY
   //Whether to enable the device certification process (confirm with the firmware engineer)
   bluetoothOption.isUseAuthDevice = JL_Constant.IS_NEED_DEVICE_AUTH
   //Set the MTU of BLE
   bluetoothOption.mtu = BluetoothConstant.BLE_MTU_MIN
   //Do you need to change the MTU of BLE
   bluetoothOption.isNeedChangeMtu = false
   //Whether to enable Jerry server (temporarily not supported)
   bluetoothOption.isUseJLServer = false
   //Do you need to adjust the MTU size of BLE (the MTU is not adjusted by default, if you need to adjust, please set it with the mtu attribute)
   bluetoothOption.isNeedChangeMtu = false
   //Configure OTA parameters
   configure(bluetoothOption)
````

## Logcat switch description

1. The switch LOG can be set using JL_Log.setIsLog(boolean bl)
2. Save the LOG to the local The premise is that the Log is open, and call JL_Log.setIsSaveLogFile(boolean bl) to set
  * If saving is enabled, remember to close the save log file before exiting the app
  *Log save location:
    * If the phone system is Android 10.0+, put it in /Android/data/com.jieli.otasdk/files/com.jieli.otasdk/logcat/
    * If the mobile phone system is below Android 10.0, put it in /com.jieli.otasdk/logcat/
    
## Version channel description

1. The Debug version enables printing by default, and you can choose the test configuration.
  * Whether to enable device authentication (default enabled)
  * Whether it is a HID device (off by default, the connection method has changed, because the HID device system will actively connect back)
  * Whether to customize the back-connection method (off by default, if you need to customize the back-connection method, you need to implement it yourself)
2. The Release version turns off printing by default and does not display the test configuration.

## LAN file transfer instructions

For the convenience of testing, a local area network transmission tool has been specially added. Easy to transfer OTA upgrade files and Log files

1. Open the [Snapdrop.net](https://snapdrop.net/) website or other Snapdrop app on any other device in your local WiFi network.

2. Tap the device you want to share the file with,

​ **App operation**: Check the Send file, click to select to send it. The default open browse folder is the upgrade folder. If you send a log file, you need to go back to the previous level and enter the logcat folder.

​ **PC side operation:** PC side supports dragging and dropping files and opening file browser selection

![LAN file transfer](doc/LAN transfer demo.gif)

## OTA automatic test instructions

In order to facilitate continuous testing, the automated test OTA function has been specially added. **Default number of tests: 10**, **One error will stop immediately**.

1. In the test settings interface, check the OTA option for automated testing, and click the save button at the top right to save the settings.

2. In the discovery interface, select the device to be upgraded.

3. On the device upgrade interface, select the upgrade file.

4. Click the [Device Upgrade] button to start the automated test.

````text
Rebooting the device to rediscovering the device may result in a longer time interval between two upgrades, but it will not affect the continuation of the automatic test, which is a normal phenomenon.
````