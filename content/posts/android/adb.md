---
title: "ADB Commands"
date: 2019-04-28T17:45:52Z
draft: false
categories: [Android]
tags: []
card: false
weight: 0
---

## What Is ADB

Android Debug Bridge (adb) is a command line tool that lets you communicate with an emulator or connected Android device. You can find the adb tool in *android sdk/platform-tools* or Download [ADB Kits](http://adbshell.com/downloads).

<!--more-->

## ADB Debugging

```
adb devices
adb forward
adb kill-server
```

### adb devices

Prints a list of all attached emulator/device

```
adb devices
```

In response, return serial number and state 

```
e4b25377        device
emulator-5554  device
```

### adb forward

forward socket connections

```
adb forward <local> <remote>
```

adb forward tcp:8000 tcp:9000 set up forwarding of host port 8000 to emulator/device port 9000

**Prerequisites: Enable USB debugging on the device.**

### adb kill-server

terminates the adb server process

```
adb kill-server
```

**Notes: kill the server if it is running. (terminal adb.exe process)**

## Wireless

```
adb connect <host>[:<port>]
adb usb
```

### adb connect


use ADB over Wi-Fi

```
adb connect <host>[:<port>]
```

#### STEP 1.

Connect to the device over USB.

#### STEP 2.

```
adb devices
```

List of devices attached
\######## device

Notes: STEP 1,2 is required
#### STEP 3.

```
adb tcpip 5555
```

restarting in TCP mode port: 5555

#### STEP 4.

Find out the IP address of the Android device: Settings -> About -> Status -> IP address. Remember the IP address, of the form #.#.#.#.

#### STEP 5.

```
adb connect #.#.#.#
```

connected to #.#.#.#:5555

#### STEP 6.

Remove USB cable from device, and confirm you can still access device:

```
adb devices
```

List of devices attached
\#.#.#.#:5555 device

**Notes: Make sure that your host is still connected to the same Wi-Fi network your Android device is.**

### adb usb

restarting ADB in USB mode.

See also: [adb connect](http://adbshell.com/commands/adb-connect)


## Package Manager

```
adb install [option] <path>
adb uninstall [options] <PACKAGE>
adb shell pm list packages [options] <FILTER>
adb shell pm path <PACKAGE>
adb shell pm clear <PACKAGE>
```

### adb install

Pushes an Android application (specified as a full path to an .apk file) to an emulator/device.

```python
adb install test.apk
adb install -l test.apk
	# forward lock application
adb install -r test.apk
	# replace existing application
adb install -t test.apk
	# allow test packages
adb install -s test.apk
	# install application on sdcard
adb install -d test.apk
	# allow version code downgrade
adb install -p test.apk
	# partial application install
```

## adb uninstall

Removes a package from the emulator/device.

```python
adb uninstall com.test.app
adb uninstall -k com.test.app
	# Keep the data and cache directories around after package removal.
```

### adb shell pm list packages

Prints all packages, optionally only those whose package name contains the text in &lt;FILTER&gt;.

```python
adb shell pm list packages
adb shell pm list packages -f
	# See their associated file.
adb shell pm list packages -d
	# Filter to only show disabled packages.
adb shell pm list packages -e
	# Filter to only show enabled packages.
adb shell pm list packages -s
	# Filter to only show system packages.
adb shell pm list packages -3
	# Filter to only show third party packages.
adb shell pm list packages -i
	# See the installer for the packages.
adb shell pm list packages -u
	# Also include uninstalled packages.
adb shell pm list packages --user <USER_ID>
	# The user space to query.
```

### adb shell pm path

Print the path to the APK of the given &lt;PACKAGE&gt;.

```
adb shell pm path com.android.phone
```

package:/system/priv-app/TeleService/TeleService.apk

### adb shell pm clear

Deletes all data associated with a package.

```
adb shell pm clear com.test.abc
```

**Notes: clearing app data, cache**

## File Manager

```
adb pull <remote> [local]
adb push <local> <remote>
adb shell ls
adb shell cd
adb shell rm
adb shell mkdir
adb shell touch
adb shell pwd
adb shell cp
adb shell mv
```

### adb pull

Download a specified file from an emulator/device to your computer.

```
adb pull /sdcard/demo.mp4
```

download /sdcard/demo.mp4  to &lt;android-sdk-path&gt;/platform-tools directory.

```
adb pull /sdcard/demo.mp4 e:\
```

download /sdcard/demo.mp4 to drive E.

### adb push

Upload a specified file from your computer to an emulator/device.

```
adb push test.apk /sdcard
```

Copies &lt;android-sdk-path&gt;/platform-tools/test.apk to /sdcard directory.

```
adb push d:\test.apk /sdcard
```

Copies d:\test.apk to /sdcard directory.

### adb shell ls

list directory contents

```
ls [options] <directory>
```

#### STEP 1.

```
adb shell
```

#### STEP 2.

```python
ls
ls -a
	# do not hide entries starting with
ls -i
	# print index number of each file
ls -s
	# print size of each file, in blocks
ls -n
	# list numeric UIDs and GIDs
ls -R
	# list subdirectories recursively
```

Notes: *press Ctrl-C to stop*

### adb shell cd

change directory

```
cd <directory>
```

#### STEP 1.

```
adb shell
```

#### STEP 2.

```
cd /system
```

### adb shell rm

remove files or directories

```
rm [options] <files or directory>
```

STEP 1.

```
adb shell
```

STEP 2.

rm /sdcard/test.txt

```python
rm -f /sdcard/test.txt
	# force remove without prompt
rm -r /sdcard/tmp
	# remove the contents of directories recursively
rm -d /sdcard/tmp
	# remove directory, even if it is a non-empty directory
```

**Notes: rm -d equal rmdir command**

```python
rm -i /sdcard/test.txt
	# prompt before any removal
```

### adb shell mkdir

make directories

```
mkdir [options] <directory name>
```

```python
mkdir /sdcard/tmp
mkdir -m 777 /sdcard/tmp
	# set permission mode
mkdir -p /sdcard/tmp/sub1/sub2
	# create parent directories as needed
```

### adb shell touch

create empty file or change file timestamps

```
touch [options] <file>
```

STEP 1.

```
adb shell
```

STEP 2.

```
touch /sdcard/tmp/test.txt
```

ls /sdcard/tmp

### adb shell pwd

print current working directory location.

```
pwd
```

### adb shell cp

copy files and directories

```
cp [options] <source> <dest>
```

STEP 1.

```
adb shell
```

STEP 2.

cp /sdcard/test.txt /sdcard/demo.txt

### adb shell mv

move or rename files

```
mv [options] <source> <dest>
```

STEP 1.

```
adb shell
```

STEP 2.

```python
mv /sdcard/tmp /system/tmp 
	# move
mv /sdcard/tmp /sdcard/test
	# rename
```

## Network

```
adb shell netstat
adb shell ping
adb shell netcfg
adb shell ip
```

### adb shell netstat

network statistics

```
netstat
```

STEP 1.

```
adb shell
```

STEP 2.

```
netstat
```

### adb shell ping

test the connection and latency between two network connection.

ping [-aAbBdDfhLnOqrRUvV] [-c count] [-i interval] [-I interface]
[-m mark] [-M pmtudisc_option] [-l preload] [-p pattern] [-Q tos]
[-s packetsize] [-S sndbuf] [-t ttl] [-T timestamp_option]
[-w deadline] [-W timeout] [hop1 ...] destination

#### STEP 1.

```
adb shell
```

#### STEP 2.

```
ping www.google.com
```

Notes: press Ctrl-C to stop ping

```
ping www.google.com -c 4
```

### adb shell netcfg

configure and manage network connections via profiles

```
netcfg [<interface> {dhcp|up|down}]
```

#### STEP 1.

```
adb shell
```

#### STEP 2.

```
netcfg
```

### adb shell ip

show, manipulate routing, devices, policy routing and tunnels

```
ip [ OPTIONS ] OBJECT
```

OBJECT := { link | addr | addrlabel | route | rule | neigh | ntable |tunnel | tuntap | maddr | mroute | mrule | monitor | xfrm |netns | l2tp }

OPTIONS := { -V[ersion] | -s[tatistics] | -d[etails] | -r[esolve] |-f[amily] { inet | inet6 | ipx | dnet | link } |-l[oops] { maximum-addr-flush-attempts } |-o[neline] | -t[imestamp] | -b[atch] [filename] |-rc[vbuf] [size]}

#### STEP 1.

```
adb shell
```

#### STEP 2.

```python
ip -f inet addr show wlan0 
	# show WiFi IP Address
```

## Logcat

```
adb logcat
adb shell dumpsys
adb shell dumpstate
```

Prints log data to the screen.

```
adb logcat [option] [filter-specs]
```

```
adb logcat
```

Notes: press Ctrl-C to stop monitor

```python
adb logcat *:V
	# lowest priority, filter to only show Verbose level
adb logcat *:D
	# filter to only show Debug level
adb logcat *:I
	# filter to only show Info level
adb logcat *:W
	# filter to only show Warning level
adb logcat *:E
	# filter to only show Error level
adb logcat *:F
	# filter to only show Fatal level
adb logcat *:S
	# Silent, highest priority, on which nothing is ever printed
```



```
adb logcat -b <Buffer>
```

```python
adb logcat -b radio
	# View the buffer that contains radio/telephony related messages.
adb logcat -b event
	# View the buffer containing events-related messages.
adb logcat -b main
	# default
adb logcat -c
	# Clears the entire log and exits.
adb logcat -d
	# Dumps the log to the screen and exits.
adb logcat -f test.logs
	# Writes log message output to test.logs .
adb logcat -g
	# Prints the size of the specified log buffer and exits.
adb logcat -n <count>
	# Sets the maximum number of rotated logs to <count>. 
```

Notes: The default value is 4. Requires the -r option.

```python
adb logcat -r <kbytes>
	# Rotates the log file every <kbytes> of output.
```

Notes: The default value is 16. Requires the -f option.

```python
adb logcat -s
	# Sets the default filter spec to silent.
```


```
adb logcat -v <format>
```

```python
adb logcat -v brief
	# Display priority/tag and PID of the process issuing the message (default format).
adb logcat -v process
	# Display PID only.)
adb logcat -v tag
	# Display the priority/tag only.
adb logcat -v raw
	# Display the raw log message, with no other metadata fields.
adb logcat -v time
	# Display the date, invocation time, priority/tag, and PID of the process issuing the message.
adb logcat -v threadtime
	# Display the date, invocation time, priority, tag, and the PID and TID of the thread issuing the message.
adb logcat -v long
	# Display all metadata fields and separate messages with blank lines.
```

### adb shell dumpsys

dumps system data

```
adb shell dumpsys [options]
```

```
adb shell dumpsys
```

adb shell dumpsys meminfo

```
adb shell dumpsys battery
```

Notes: A mobile device with Developer Options enabled running Android 5.0 or higher.

```python
adb shell dumpsys batterystats
	# collects battery data from your device
```

**Notes: [Battery Historian](https://github.com/google/battery-historian) converts that data into an HTML visualization. ** STEP 1 *adb shell dumpsys batterystats > batterystats.txt* **STEP 2** *python historian.py batterystats.txt > batterystats.html*

```python
adb shell dumpsys batterystats --reset
	# erases old collection data
```

adb shell dumpsys activity

adb shell dumpsys gfxinfo com.android.phone *measuring com.android.phone ui performance*

### adb shell dumpstate

dumps state

```python
adb shell dumpstate
adb shell dumpstate > state.logs
	# dumps state to a file
```


## Screenshot

```
adb shell screencap
adb shell screenrecord [4.4+]
```

### adb shell screencap

taking a screenshot of a device display.

```
adb shell screencap <filename>
```

```
adb shell screencap /sdcard/screen.png
```

download the file from the device

```
adb pull /sdcard/screen.png
```

### adb shell screenrecord

recording the display of devices running Android 4.4 (API level 19) and higher.

```
adb shell screenrecord [options] <filename>
```

```
adb shell screenrecord /sdcard/demo.mp4
```

(press Ctrl-C to stop recording)

download the file from the device

```
adb pull /sdcard/demo.mp4
```

**Notes: Stop the screen recording by pressing Ctrl-C, otherwise the recording stops automatically at three minutes or the time limit set by --time-limit.**

```
adb shell screenrecord --size <WIDTHxHEIGHT>
```

Sets the video size: 1280x720. The default value is the device's native display resolution (if supported), 1280x720 if not. For best results, use a size supported by your device's Advanced Video Coding (AVC) encoder.

```
adb shell screenrecord --bit-rate <RATE>
```

Sets the video bit rate for the video, in megabits per second. The default value is 4Mbps. You can increase the bit rate to improve video quality, but doing so results in larger movie files. The following example sets the recording bit rate to 5Mbps: *adb shell screenrecord --bit-rate 5000000 /sdcard/demo.mp4*

```
adb shell screenrecord --time-limit <TIME>
```

Sets the maximum recording time, in seconds. The default and maximum value is 180 (3 minutes).

```
adb shell screenrecord --rotate
```

Rotates the output 90 degrees. This feature is experimental.

```
adb shell screenrecord --verbose
```

Displays log information on the command-line screen. If you do not set this option, the utility does not display any information while running.

## System

```
adb rootadb sideload
adb shell ps
adb shell top
adb shell getprop
adb shell setprop
```

### adb root

restarts the adbd daemon with root permissions

```
adb root
```

Notes: adbd cannot run as root in production builds (test in emulator)

### adb sideload

flashing/restoring Android update.zip packages.

```
adb sideload <update.zip>
```

Notes: adb reboot sideload *[Android M+]*

### adb shell ps

print process status

```
ps [options]
```

#### STEP 1.

```
adb shell
```

#### STEP 2. 

```
ps
```

ps -p

### adb shell top

display top CPU processes

```
top [options]
```

#### STEP 1.

```
adb shell
```

#### STEP 2.

```
top
```

Notes: (press Ctrl-C to stop monitor)

top -t *Show threads instead of processes.*

### adb shell getprop

get property via the android property service

```
getprop [options]
```

#### STEP 1.

```
adb shell
```

#### STEP 2.

```
getprop
getprop ro.build.version.sdk
getprop ro.chipname
getprop | grep adb
```

See Also [adb shell setprop](http://adbshell.com/commands/adb-shell-setprop)

### adb shell setprop

set property service

```
setprop <key> <value>
```

#### STEP 1.

```
adb shell 
```

#### STEP 2.

setprop service.adb.tcp.port 5555

See Also [adb shell getprop](http://adbshell.com/commands/adb-shell-getprop)

