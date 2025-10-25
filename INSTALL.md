check arch

```bash
uname -m
```

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

archv8l
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



Architecture  |  Type  |  Notable Use Cases          |  Example ISA   
--------------+--------+-----------------------------+----------------
x86 / AMD64   |  CISC  |  PCs, servers               |  Intel/AMD     
ARM           |  RISC  |  Mobile, IoT, embedded      |  ARMv8, ARMv9  
MIPS          |  RISC  |  Routers, embedded devices  |  MIPS64        
PowerPC       |  RISC  |  Industrial, embedded       |  PPC64         
SPARC         |  RISC  |  Servers, HPC               |  SPARCv9       
IBM s390/x    |  CISC  |  Mainframes                 |  z/Architecture

</details>

AArch64, also known as ARM64, is the 64-bit execution state of the ARM processor architecture family. It was first introduced with ARMv8-A in 2011 and remains the foundation for ARMv9 processors used in modern smartphones, tablets, laptops, and servers.


