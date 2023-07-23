<h1>build hello_world with cross compiler</h1>
<h2>download cross compiler</h2>
<p>
Go to Texas Instruments webside to download SDK and Toolchains: <br>
    <a href="https://www.ti.com/tool/PROCESSOR-SDK-AM335X?keyMatch=AM335X#downloads">Processor SDK AM335X</a> <br>
    <a href="https://developer.arm.com/-/media/Files/downloads/gnu-a/9.2-2019.12/binrel/gcc-arm-9.2-2019.12-x86_64-arm-none-linux-gnueabihf.tar.xz">Toolchain for AM335X Download</a><br>
    <a href="https://dr-download.ti.com/software-development/software-development-kit-sdk/MD-1BUptXj3op/08.02.00.24/am335x-evm-linux-sdk-src-08.02.00.24.tar.xz">AM335x SDK Download</a> <br>

</p>
<p>
untar Toolchain with command:

```
tar -xvf gcc-arm-9.2-2019.12-x86_64-arm-none-linux-gnueabihf.tar.xz
```
Export cross-compliter to enviromment variable $CC
```
export CC=/mnt/d/wsl_export/TI_Source/gcc-arm-9.2-2019.12-x86_64-arm-none-linux-gnueabihf/bin
```
</p>
<h2>hello_world program</h2>
<p>
C hello world program:

```
#include <stdio.h>

void main()
{
    printf("hello world\r\n");
}
```
</p>
<p>
build program:

```
$CC/arm-none-linux-gnueabihf-gcc main.c -o hello_world
```
</p>