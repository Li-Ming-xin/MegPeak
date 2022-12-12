# MegPeak
Megpeak is a tool for testing processor peak computation, now support
arm, x86 and GPU driven by OpenCL processor.

## MegPeak can do
MegPeak can do:
* Peak bandwidth of instruction
* Instruction delay
* Memory peak bandwidth
* Peak bandwidth of arbitrary instruction combination

Although some of the above information can be obtained by querying the data sheet of the chip, and with guidance the theoretical the peak performance
can be computed, but in many cases, the detail performance documents of the target processor cannot be obtained.
In addition, the result through megpeak is more direct and accurate, and it can test the peak bandwidth of arbitrary instruction combination

## BUILD
MegPeak only support [CMake](https://cmake.org/) build system and require CMake version upper than 3.15.2, you can compile the MegPeak follow the step:

* clone or download the project
* choose a test platform
    - if you will test x86 processor in linux OS
        - a gcc or clang compiler should find by cmake through `PATH` env
    - if you will test arm processor in android OS
        - a [ndk](https://developer.android.com/ndk) is required
            - download the NDK and extract to the host machine
            - set the `NDK_ROOT` env to the path of extracted NDK directory
* if your target test OS is android，run the `android_build.sh` to build it
    * build for armv7
        ```bash
        ./android_build.sh -m armeabi-v7a
        ```
    * build for arm64
        ```bash
        ./android_build.sh -m arm64-v8a
        ```
    * build with OpenCL
        ```bash
        ./android_build.sh -l -m [arm64-v8a, armeabi-v7a]
        ```
    * build with all benchmark
        ```bash
        ./android_build.sh -a -m [arm64-v8a, armeabi-v7a]
        ```
* if you target test OS is linux，if you want to enable OpenCL add `-DMEGPEAK_ENABLE_OPENCL=ON` to cmake command
    ```bash
    mkdir -p build && cd build
    cmake .. [-DMEGPEAK_ENABLE_OPENCL=ON]
    make
    ```
* after build, the executable file megpeak is stored in build directory

### Run
If you compile the project and get the megpeak, next you can copy or set the megpeak executable file to the test machine，and run it and get the help message.
    ```bash
    ./megpeak -h
     ```
* test opencl
    ```bash
    ./megpeak -d opencl
    ```
* test CPU with cpu id
    ```bash
    ./megpeak -d cpu -i 0
    ```

### GFlops test results for different CPUs
| Platform | CPU | Architecture | Frequence(GHz) | GFLOPS | FLOPS/Cycle |
| :------: | :---: | :------------: | :--------------: | :------: | :----------------: |
| / | Cortex-A7 | ARMV7 | 1.0 | 1.902002 | 1.9020 |
| / | Cortex-A53 | ARM64 | 1.3 | 9.844987 | 7.5731 |
| Xiaomi Mi 9 Core 0 | Cortex-A55 | ARM64 | 1.8 | 13.566583 | 7.537 |
| raspberry-pi 4b | Cortex-A72 | ARM64 | 1.5 | 11.664233 | 7.776 |
| Xiaomi Mi 9 Core 6 | Cortex-A76 | ARM64 | 2.42 | 38.691399 | 15.988 |
| Realme X7 Pro Core 4 | Cortex-A77 | ARM64 | 2.6 | 41.318005 | 15.892 |
| Xiaomi Mi 11 Core 6 | Cortex-A78 | ARM64 | 2.4 | 33.781448 | 14.076 |
| Apple Mac | Apple M1 | ARM64 | 3.2 | 102.424 | 32.008 |
More details see [wiki](https://github.com/MegEngine/MegPeak/wiki/Common-Arm-processor-Peak-performance)

### Acknowledgement
OpenCL in MegPeak referenced the [clpeak](https://github.com/krrishnarraj/clpeak) project

### License
[Apache-2.0](https://github.com/MegEngine/MegPeak/blob/main/LICENSE)
