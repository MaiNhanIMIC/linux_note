<h1>cài đặt một số package để build document</h1>
<p>

```
sudo apt-get install texlive-xetex
```
```
sudo apt-get install sphinx-doc
```
</p>


<h1>Tìm hiểu kernel là gì</h1>
<h2>Bootloader: U-Boot là gì?</h2>
<p>
Là một chương trình nhỏ có chức năng load hệ điều hành vào trong bộ nhớ.
Nó chính xác là một bootloader <br>
<b>U-Boot</b> là tên của một loại bootloader:<br>
 -  Nó là mã nguồn mở<br>
 - Được sử dụng rộng rãi trong các hệ thống nhúng nhỏ. Nó hỗ trợ nhiều kiến trúc. vd: 68k, ARM, Blackfin, MicroBlaze, MIPS, Nios, SuperH, PPC, RISC-V
</p>
<h3>Boot process</h3>
<h4>Bước 1: run code in boot rom</h4>
<p>
 - Khi hệ thống khởi động lần đầu tiên, hoặc reset. Quyền kiểm soát hệ thống sẽ thuộc về reset vector, nó là một đoạn mã assembly được ghi trước bởi nhà sản xuất chip. <b>boot rom</b> --> giống như vùng nhớ flash memory trong STM32F411<br>
 - Chức năng chính của FW lưu trong <b>boot rom</b> đấy chính là sao chép nội dung trong file "MLO" hay còn được gọi là <b>Second Program Loader (SPL)</b> - chương trình tải phụ vào IRAM và excute nó.<br>
 - Do bộ nhớ của boot rom khá nhỏ nên rom code cũng được giới hạn ở việc khởi tạo một số phần cứng cần thiết cho việc load SPL lên hệ thống như: MMC/eMMC, SDcard, NAND flash. Các phần cứng này được gọi chung là boot device.

--> tốm lại: <br>
Khi cấp điện vào PMU -> boot from boot rom (khởi tạo MMC/eMMC, SD card, NAND, Flash... và copy SPL lên RAM để execute) -> run SPL in RAM
<h4>Bước 2: run code in SPL (second program loader)</h4>
<p>
SPL: Nhiệm vụ chính của nó là tiếp tục setup các thành phần cần thiết như DRAM controler, eMMC vv.. Sau đó load <b>U-boot</b> tới địa chỉ <b>CONFIG_SYS_TEXT_BASE</b> của RAM.
</p>
<h4>Bước 3: U-Boot chạy</h4>
<p>
- Sau khi được load vào RAM, u-boot sẽ thực hiện việc relocation. Di dời đến địa chỉ relocaddr của RAM (Thường là địa chỉ cuối của RAM) và nhảy đến mã của u-boot sau khi di dời.<br>
- Lúc này u-boot sẽ kiểm tra xem file <b>uEnv.txt</b> có tồn tại hay không. Nếu có thực hiện load nó vào RAM ở bước tiếp theo. <br>
<lo> *** <b>uEnv.txt</b>: là một bootscript:<br>
    <li>Định nghĩa các tham số cấu hình</li>
    <li>kernel parameters</li>
    <li>...</li>
    --> các thông số này đã được default trong u-boot, tuy nhiên chúng ta có thể add thêm hoặc modify chúng. <br>
    --> Bước kiểm tra file <b>uEnv.txt</b> là optional, không bất buộc phải có file này.<br>
</lo>
- u-boot sẽ load kernel, device-tree vào RAM tại các địa chỉ được cấu hình từ trước trong mã nguồn u-boot (<b>lúc build u-boot</b>) hoặc trong file uEnv.txt. <br>
- Tiếp theo u-boot sẽ truyễn các parameter vào cho kernel. và execute kernel.

<h4>Build U-boot</h4>
<p>
Sử dụng source u-boot của TI homepage:
<a href="https://dr-download.ti.com/software-development/software-development-kit-sdk/MD-1BUptXj3op/08.02.00.24/am335x-evm-linux-sdk-src-08.02.00.24.tar.xz">click vào đây để download</a> <br>
</p>

<p>các package cần thiết cho quá trình build u-boot</p>

```
sudo apt install bison build-essential flex swig
```
<p>cần phải chỉ cho máy tính biết cross-compiler nằm ở đâu trong máy tính</p>

```
export CC=<cross-compiler path>/arm-linux-gnueabi-
```
<p>Configure and Build</p>

```
make ARCH=arm CROSS_COMPILE=${CC} distclean
make ARCH=arm CROSS_COMPILE=${CC} am335x_evm_defconfig
make ARCH=arm CROSS_COMPILE=${CC}
```
<h2>Linux kernel</h2>
</p>
- Sau khi nhận được quyền kiểm soát và các kernel parameters từ u-boot <br>
- Kernel sẽ thực hiện mount hệ thống file system (Rootfs) và cho chạy tiến trình Init trên RAM --> Đây là tiến trình được chạy đầu tiên khi hệ thống khởi động thành công và chạy cho tới khi hệ thống kết thúc <br>
- Tiến trình Init sẽ khởi tạo toàn bộ các tiến trình con khác trên user space, các applications tương tác trực tiếp với người dùng --> Lúc này, hệ thống của chúng ta đã hoàn toàn sẵn sàng cho việc sử dụng.
</p>
<h3>Build kernel</h3>
<p>
Sử dụng source kernel của TI homepage:
<a href="https://dr-download.ti.com/software-development/software-development-kit-sdk/MD-1BUptXj3op/08.02.00.24/am335x-evm-linux-sdk-src-08.02.00.24.tar.xz">click vào đây để download</a> <br>
<p>
các package cần dùng:

```
libgmp3-dev
libmpc-dev
libssl-dev
bc
```
</p>
<p>
sử dung auto script:

```
make ARCH=arm CROSS_COMPILE=${CC}/arm-none-linux-gnueabihf- beaglebone_defconfig
make ARCH=arm CROSS_COMPILE=${CC}/arm-none-linux-gnueabihf- uImage dtbs
make ARCH=arm CROSS_COMPILE=${CC}/arm-none-linux-gnueabihf- uImage-dtb.am335x-boneblack
make ARCH=arm CROSS_COMPILE=${CC}/arm-none-linux-gnueabihf- modules
```
</p>

</p>
<h3>Các thành phần trong kernel</h3>
<p>1. <b>uImage</b> or <b>zImage</b>(tùy kiểu nén): gồm nhân hệ điều hành + các build-in module (các module được khởi chạy cùng lúc kernel) </p>
<p>2. Các module khác. Không được tích hợp vào kernel, chỉ được chạy khi user execute</p>
<p>--> các module thực hiện một chức năng nào đó, hoặc có thể là một <b>driver</b> của một cái peripherial nào đó</p>
<h2>BSP là gì</h2>
<p>BSP là board support package - </p>
<p></p>
<h2>Root File System là gì</h2>
<p>Rootfs là hệ thống file, thể hiện trực quan nhất với user. các file bạn thấy và bạn truy cập tới điều nằm trong Rootfs</p>
<p>tham khảo file <a href ="Cấu Trúc Thư Mục.docx"> "Cấu Trúc Thư Mục.docx"</a> để biết cấu trúc hệ thống trong Rootfs</p>

