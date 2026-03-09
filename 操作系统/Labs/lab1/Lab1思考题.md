### RISC-V 汇编与调用约定

- 每个函数的开头都操作了 `sp`，这是在干什么？

栈指针，用于分配栈空间（用于保存返回地址、中间变量、临时保存的寄存器）

- 尝试修改 C 语言代码，你会发现 `sp` 的差值总是 16 的倍数，这是为什么？

RISC-V调用规定，栈必须16字节对齐，这样CPU访问对齐的内存更快

- 调用函数前后做了什么？

先准备参数，把参数放到a0
执行调用，跳转到call function，并将下一条指令保存到return address

调用后，分配栈空间，一通调用

最后释放栈空间，并返回



- 下列伪指令分别对应什么真实指令？

  ```
  la nop li mv j ret call tail
  ```

| 伪指令          | 功能                                                         | 对应的真实指令（典型情况）                                   |
| --------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| `la rd, symbol` | Load Address：将符号（如标签、全局变量）的地址加载到寄存器 `rd` | `auipc rd, upper20(symbol)` `addi rd, rd, lower12(symbol)`   |
| `nop`           | No Operation：空操作（什么都不做）                           | `addi x0, x0, 0`（写入零寄存器，无副作用）                   |
| `li rd, imm`    | Load Immediate：将立即数 `imm` 加载到 `rd`                   | 若 `imm` 在 -2048~2047 范围内： `addi rd, x0, imm` 否则： `lui rd, upper20(imm)` `addi rd, rd, lower12(imm)` |
| `mv rd, rs`     | Move：将 `rs` 的值复制到 `rd`                                | `addi rd, rs, 0`                                             |
| `j label`       | Jump：无条件跳转到 `label`                                   | `jal x0, label`（不保存返回地址）                            |
| `ret`           | Return：从函数返回                                           | `jalr x0, ra, 0`（即 `jr ra`）                               |
| `call label`    | 调用函数（可调用任意地址的函数）                             | `auipc ra, upper20(label)` `jalr ra, ra, lower12(label)`     |
| `tail label`    | 尾调用（tail call）：跳转到另一个函数并**不返回**            | `auipc t0, upper20(label)` `jalr x0, t0, lower12(label)`     |



· `call` 伪指令做了什么工作？它与 `tail` 指令有什么区别？



MIE，SIE

xIE：中断的强制使能
