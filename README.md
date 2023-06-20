# Bitcoin-QML-WSL-Ubuntu2204
installation guide to build Bitcoin Core QML APKs using WSL Ubuntu 22.04 (Windows 11)

## Building APKs Using WSL Ubuntu 20.04 (Windows 11) Guide

1. Install WSL Ubuntu 22.04 (Windows 11) guide [here](https://ubuntu.com/tutorials/install-ubuntu-on-wsl2-on-windows-11-with-gui-support#1-overview)
2. Once installed run Ubuntu 22.04 and update
```
sudo apt update
sudo apt upgrade
```
3. Install Bitcoin Core dependencies
```bashsudo apt update
sudo apt upgrade
sudo apt install build-essential libtool autotools-dev automake pkg-config bsdmainutils curl git
```
and QML specific dependencies
```
sudo apt install qtdeclarative5-dev qtquickcontrols2-5-dev
```
3. Install Android Studio and Android NDK (guide [here](https://linuxhint.com/install-android-studio-ubuntu22-04/))
``` 
sudo apt install openjdk-11-jdk
```
check version
```
java --version
```
install Android Studio using Snap
```
sudo snap install android-studio --classic
```
run Android Studio
```
android-studio
```
Install Android NDK full guide [here](https://developer.android.com/studio/projects/install-ndk)

1. With a project open, click Tools > SDK Manager.

2. Click the SDK Tools tab.

3. Select the NDK (Side by side) and CMake checkboxes.
4. Click OK.

5. A dialog box tells you how much space the NDK package consumes on disk.

6. Click OK.

7. When the installation is complete, click Finish.

Before building the apks first check your Android Device hardware architecture
using usbip (guide [here](https://www.xda-developers.com/wsl-connect-usb-devices-windows-11/)) and adb

1. Connect your android device to your PC and enable USB debugging in Developer Options.
2. Once usbip is installed, run the following command as Administrator in `cmd` to list all the USB devices connected to your PC.
```
usbipd wsl list
```
3. You will see a list of all the USB devices connected to your PC. Note down the `BUSID` of the device you want to connect to WSL. Then run, replacing `<busid>` with the `BUSID` of your device.
```
usbipd wsl attach --busid <busid>
```
4. install adb
```
sudo apt install adb
```
5. then run
```
adb shell getprop ro.product.cpu.abi
```
6. check the output and note down the architecture (arm64-v8a, armeabi-v7a, x86_64, x86)



Also don't forget to run the following command before building the depends (more details [here](https://github.com/bitcoin/bitcoin/blob/master/doc/build-windows.md#compiling-with-windows-subsystem-for-linux))
```
PATH=$(echo "$PATH" | sed -e 's/:\/mnt.*//g') # strip out problematic Windows %PATH% imported var
```
```
sudo bash -c "echo 0 > /proc/sys/fs/binfmt_misc/status" # Disable WSL support for Win32 applications.
```
Once that's done you can build the apks using the following guide [here](https://github.com/bitcoin-core/gui-qml/blob/main/doc/build-android.md)
