<h1>PROCESSES</h1>
<h2>1. Processes and Programs</h2>
<p>
    - Một procces thể thiện một program đang được execute.<br>
    - Một program là một file chứa machine-language intructions, chứa mã lệnh thực hiện chương trình.<br>
</p>

<h2>2. Process ID and Perent Process ID</h2>
<p>
    - Mỗi một process sẽ được cấp 1 ID (PID)<br>
</p>

    #include <unistd.h>
    pid_t getpid(void);

the ```getid()``` hệ thống sẽ trả về proccess ID của proccess được gọi.
<h2>3. Memory layout of a process</h2>
<p>
    - Memory được phân bổ cho mỗi process bao gồm một số path. Giống như trong lớp C/C++. Bao gồm: code signment, data, bss, stack, heap.
</p>

<h2>4. Virtual memory memagement</h2>
<p>
    - spatical (không gian) locality:
</p>
<p>
    - temporal (tạm thời) locality:
</p>

5. Stack and stack frames
6. Command-line arguments(argc, argv)
7. Enviroment list
8. Performing a onlocal goto: setjmp() and long jmp()