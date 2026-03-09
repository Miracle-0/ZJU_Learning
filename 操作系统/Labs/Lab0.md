# Lab0

## 新工具

1. docker：镜像（先把环境丢进去）+容器（配好的隔离环境）

## 提问

### 容器与镜像的关系：

镜像是食谱，容器是做出来的菜。镜像是配好的环境，容器是部署到本地以后的东西。

### 使用交叉编译工具链

riscv64-linux-gnu-gcc -S hello.c -o hello.s 生成汇编代码（交叉编译器）

riscv64-linux-gnu-gcc hello.c -o hello_riscv 编译为 RISC-V 可执行程序

riscv64-linux-gnu-objdump -d hello_riscv > hello_disasm.txt 反汇编 RISC-V 程序

### 编译本地架构内核

`defconfig`：用于生成config内核配置文件

`distclean`：彻底清理配置环境

如何开启构建过程的详细输出？没学

4.3：核心是配置 3 个关键变量（`ARCH`目标架构、`CROSS_COMPILE`调用工具的前缀、`INSTALL_MOD_PATH`内核模块的安装路径）

4.4 使用 QEMU 运行 RISC-V 内核

- 查看寄存器（info registers）、内存树(info mtree 主要分为三种空间：I/O，CPU-memory-0, memory)、设备树(info qtree 定时器设备，中断控制器等)、物理内存中的值
- Linux 第一条指令位于物理内存 `0x80200000`，打印这条指令(xp /1i 0x80200000)

4.7 

- `make run` 和 `make debug` 有什么不同？新增的选项含义是什么？（多了个-s和-S，一个开启了GDB调试，一个是会中断CPU）
- 你运行 `make gdb` 时，脚本对 GDB 做了哪些设置？（`target extended-remote :1234`：建立 “增强型远程调试连接”，`layout split`：开启 “分屏调试视图”）

```bash
make ARCH=riscv defconfig
```