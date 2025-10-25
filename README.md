# Compile swift code on Linux and run it on Android device

### Install Swift using swiftly

https://www.swift.org/install/linux/

```bash
curl -O https://download.swift.org/swiftly/linux/swiftly-$(uname -m).tar.gz && \
tar zxf swiftly-$(uname -m).tar.gz && \
./swiftly init --quiet-shell-followup && \
. "${SWIFTLY_HOME_DIR:-$HOME/.local/share/swiftly}/env.sh" && \
hash -r
```

### Install tool chain

```bash
swiftly install main-snapshot-2025-10-16
```
### Update the global default toolchain
```bash
swiftly use main-snapshot-2025-10-16
```

```bash
❯ swiftly install main-snapshot-2025-10-16
Installing Swift main-snapshot-2025-10-16
Installing package in user home directory...
main-snapshot-2025-10-16 installed successfully!

❯ swiftly use main-snapshot-2025-10-16
The global default toolchain has been set to `main-snapshot-2025-10-16` (was 6.2.0)

❯ swiftly run swift --version
Apple Swift version 6.3-dev (LLVM 0d0246569621d5b, Swift 199240b3fe97eda)
Target: arm64-apple-macosx15.0
```

### Install the Swift SDK for Android

```bash
swift sdk install https://download.swift.org/development/android-sdk/swift-DEVELOPMENT-SNAPSHOT-2025-10-16-a/swift-DEVELOPMENT-SNAPSHOT-2025-10-16-a_android-0.1.artifactbundle.tar.gz --checksum 451844c232cf1fa02c52431084ed3dc27a42d103635c6fa71bae8d66adba2500
```

### Check SDK installtion status ($ swift sdk list)
```bash
swiftly run swift sdk list
```

```bash
❯ swiftly run swift sdk list
swift-DEVELOPMENT-SNAPSHOT-2025-10-17-a-android-0.1
```

### Install and configure the Android NDK

Swift SDK for Android depends on the Android NDK version 27d

```bash
mkdir ~/android-ndk
cd ~/android-ndk
curl -fSLO https://dl.google.com/android/repository/android-ndk-r27d-$(uname -s).zip
unzip -q android-ndk-r27d-*.zip
echo 'export ANDROID_NDK_HOME=$HOME/android-ndk/android-ndk-r27d' >> ~/.bashrc
source ~/.bashrc
```

### Install Andoid command line tools

Download latest command line tools from https://developer.android.com/studio

(Look for "Command line tools only" section on webpage)

```bash
unzip commandlinetools-linux*.zip
sudo mv cmdline-tools/ /opt/
echo 'export PATH=$PATH:/opt/cmdline-tools/bin' >> ~/.bashrc
source ~/.bashrc
```

### Install OpenJDK
```bash
sudo apt install openjdk-17-jdk -y
echo 'export JAVA_HOME=$(dirname $(dirname $(readlink -f $(which javac))))' >> ~/.bashrc
source ~/.bashrc
```

### Install platform tools like adb, fastboot and etc.

```bash
mkdir $HOME/Android
./sdkmanager --sdk_root=$HOME/Android "platform-tools"
echo 'export PATH=$PATH:$HOME/Android/platform-tools' >> ~/.bashrc
source ~/.bashrc
```

### Link NDK to the Swift SDK for Android

```bash
cd ~/.config/swiftpm/swift-sdks/swift-DEVELOPMENT-SNAPSHOT-*/swift-android/scripts
./setup-android-sdk.sh
```


## ** Configured fully working cross-compilation toolchain for Android. **

<br><br><br>

# Create Hello world command line tool

### Create swift package

```bash
mkdir ~/Work
cd ~/Work
mkdir hello
cd hello

# New swift package
swiftly run swift package init --type executable

```

### Build package on host machine (linux)

```bash
swiftly run swift build
```

### Run the binary on host machine
```bash
.build/debug/hello
```


## Now check Android device processor architecture for creating binary file



### 1. check Android architecture in Termux

```bash
uname -m
```

### 2. Arch can be verified using adb

1. Enable Developer mode on Android
2. Connect Android to computer via USB capabilities
3. Approve access permission popup on Android
4. Choose 'File Transfer' from Android notifications
5. Check if device is connected.

```bash
adb devices
```
6. check Android architecture using adb
```bash
adb shell uname -m
```

<br><br><br>

### Check supported SDK for armv8l (ARM 32 bit processor)

<details>
  <summary>What is armv8l?</summary>

| Part    | Meaning                  | Explanation                                                                                                                                                |
| ------- | ------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **arm** | **Architecture family**  | Refers to ARM CPUs — the *Reduced Instruction Set Computing (RISC)* family developed by ARM Ltd.                                                           |
| **v8**  | **Architecture version** | Indicates **ARMv8**, the 8th generation of the ARM architecture. Introduced 64-bit support (AArch64) along with backward-compatible 32-bit mode (AArch32). |
| **l**   | **Endianness indicator** | The letter `l` stands for **little-endian**, meaning the least significant byte is stored first in memory.                                                 |


Context: Why it sometimes shows up as armv8l (not armv8a)

armv8a → official architecture name (A-profile CPUs, supports 64-bit AArch64).

armv8l → what Linux reports when the CPU supports ARMv8, but the kernel and userland are running in 32-bit (AArch32) mode.

In other words:

armv8l = ARMv8 hardware, 32-bit operating mode (little-endian)

So armv8l essentially describes a 32-bit ARMv8 little-endian runtime environment — a 64-bit CPU running 32-bit code.

</details>

```bash
cd ~/.config/swiftpm/swift-sdks/swift-DEVELOPMENT-SNAPSHOT-2025-10-17-a-android-0.1.artifactbundle/swift-android
grep "\"arm" swift-sdk.json
```

```bash
    "armv7-unknown-linux-android28": {
    "armv7-unknown-linux-android29": {
    "armv7-unknown-linux-android30": {
    "armv7-unknown-linux-android31": {
    "armv7-unknown-linux-android32": {
    "armv7-unknown-linux-android33": {
    "armv7-unknown-linux-android34": {
    "armv7-unknown-linux-android35": {
    "armv7-unknown-linux-android36": {
```


<details>
  <summary>Major Processor Architecture Types</summary>


#### Major Processor Architecture Types

1. RISC (Reduced Instruction Set Computing)
2. CISC (Complex Instruction Set Computing)
3. x86 Architecture
 	- Developed by Intel
 	- x86 is a CISC-based architecture dominating PCs and workstations
 	- 64-bit variant, known as x86-64 or AMD64
 	- x86-64 or AMD64, extends memory addressing capabilities and performance while maintaining backward compatibility with 32-bit systems
4. ARM Architecture
	- ARM (Advanced RISC Machine) is a widely used RISC-based architecture 
	- energy efficiency and scalability
	- It exists in multiple profiles
		a) A-Profile: For high-performance applications (e.g., smartphones, laptops).
		b) R-Profile: For real-time and safety-critical systems.
		c) M-Profile: For microcontrollers and IoT devices.​
5. MIPS Architecture
6. PowerPC Architecture
7. SPARC Architecture
8. IBM System/390 (s390/s390x) for mainframes

| Architecture | Type | Notable Use Cases         | Example ISA      |
|---------------|------|---------------------------|------------------|
| x86 / AMD64   | CISC | PCs, servers              | Intel/AMD        |
| ARM           | RISC | Mobile, IoT, embedded     | ARMv8, ARMv9     |
| MIPS          | RISC | Routers, embedded devices | MIPS64           |
| PowerPC       | RISC | Industrial, embedded      | PPC64            |
| SPARC         | RISC | Servers, HPC              | SPARCv9          |
| IBM s390/x    | CISC | Mainframes                | z/Architecture   |



</details>

AArch64, also known as ARM64, is the 64-bit execution state of the ARM processor architecture family. 

It was first introduced with ARMv8-A in 2011 and remains the foundation for ARMv9 processors used in modern smartphones, tablets, laptops, and servers.




<br><br><br>

### Enable ADB via Wifi

(Assumption: Developer mode is already enabled in Andoid)

1. Connect android and computer via USB cable.
2. Unlock Andoid and choose 'File Transfer' from android notifications.
3. Check device status in Linux Terminal
```bash
adb devices
```

```bash
❯ adb devices
List of devices attached
GMXXXNW400000G6KL        device
```

4. Enable TCP/IP mode
```bash
adb tcpip 5555
```

```bash
❯ adb tcpip 5555
restarting in TCP mode port: 5555

```
5. Connect both device in same network (Wifi)
6. Get Android device local IP

```bash
adb shell ip route
```

```bash
❯ adb shell ip route
192.168.1.0/24 dev wlan0 proto kernel scope link src 192.168.1.3
```
7. Connect wirelessly
```bash
adb connect 192.168.1.3:5555
```

```bash
❯ adb connect 192.168.1.3:5555
connected to 192.168.1.3:5555
```
8. Verify device status
```bash
adb devices
```

```bash
❯ adb devices
List of devices attached
GMXXXNW400000G6KL        device
192.168.1.3:5555        device
```

9. Unplug the USB connector

10. Disconnect wireless connection
```bash
adb disconnect 192.168.1.3:5555
```


## Next: Build and install binary on Android

### Build for Android device architecture

```bash
swiftly run swift build --swift-sdk armv7-unknown-linux-android28 --static-swift-stdlib
```

Simulator - x86_64-unknown-linux-android28

64 Bit Aarch64 - aarch64-unknown-linux-android28

32 bit Arm processor - armv7-unknown-linux-android28

### Check binary file type
```bash
file .build/armv7-unknown-linux-android29/debug/hello
```

```bash
❯ file .build/armv7-unknown-linux-android29/debug/hello

.build/armv7-unknown-linux-android29/debug/hello: ELF 32-bit LSB pie executable, ARM, EABI5 version 1 (SYSV), dynamically linked, interpreter /system/bin/linker, BuildID[sha1]=cfc06ed8e6c1d27b550182d129d1b7e911caeb94, with debug_info, not stripped
```


### Copy binary and respective "libc++_shared.so" file to Android device

```bash
adb push .build/armv7-unknown-linux-android29/debug/hello /data/local/tmp
adb push $ANDROID_NDK_HOME/toolchains/llvm/prebuilt/*/sysroot/usr/lib/arm-linux-androideabi/libc++_shared.so /data/local/tmp
```

### Run binary in Android device
```bash
adb shell /data/local/tmp/hello
```

