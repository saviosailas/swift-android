### check Android architecture in Termux

```bash
uname -m
```

### Arch can be verified using adb

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

armv8l

armv7-unknown-linux-android28

armv7-unknown-linux-android..

armv7-unknown-linux-android36


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


### Install

