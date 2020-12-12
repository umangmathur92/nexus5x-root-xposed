
# Rooting the Nexus 5X AVD and installing Xposed


**Link to Video: https://youtu.be/67kigArRQw0**


This process has 2 Parts:

**1) Creating and Rooting an Android Studio AVD - Nexus 5X, Android 7.1.1 (Nougat, API 25)**

* From Android Studio, Open up the Android Virtual Device Manager (Tools -> AVD Manager)
* Click on 'Create Virtual Device', Select Nexus 5X from the list
* Choose an x86 System Image for Nougat (API Level 25)
* The x86 Image's target should be either "Android 7.1.1 (Google APIs)" or "Android 7.1.1"
* The System Image with target "7.1.1 (Google Play)" can not be rooted
* Set AVD Name: "MyAVD" to complete the AVD creation process
* Start emulator (You may chooose to remove the `-enable-kvm` argument if your machine doesn't support this feature):
  
  `/path/to/android/sdk/emulator/emulator -avd MyAVD -writable-system -selinux permissive -qemu -enable-kvm`
* Wait for boot to complete
* Restart adbd as root and remount system as writable:
  
  `adb root && adb remount`
* Install Superuser.apk:
  
  `adb install SuperSU/common/Superuser.apk`
* Push su and update permissions:
  
  `adb push SuperSU/x86/su /system/xbin/su`
  
  `adb shell chmod 0755 /system/xbin/su`
* Set SELinux Permissive: 
  
  `adb shell setenforce 0`
* Install SuperSU's su to system: 
  
  `adb shell su --install`
* Run SuperSU's su as daemon: 
  
  `adb shell su --daemon&`
* Open the SuperSU app on the AVD, and it will prompt you to update the 'su' binary - Click `Accept` and use `Normal` installation
* Installation will fail - no need to worry, just exit the app. It will still work.
* The AVD is now rooted with SuperSU

Note: Superuser may not always persist after reboot. This can be remedied easily
* Run SuperSU's su as daemon: 
  
  `adb shell su --daemon&`
* Root should now work again


**2) Installing Xposed Framework v89 (Using XposedInstaller_3.1.5.apk)**

* Install Xposed on the AVD using the XposedInstaller APK file
  
  `adb install XposedInstaller_3.1.5.apk`
* Open the 'Xposed' Installer/Manager App and click on Install
* Grant the Superuser request when prompted
* Reboot when prompted by the UI
* Wait for reboot to complete
* Open the 'Xposed' Manager App. You should be able to see text indicating that Xposed Version 89 is installed


