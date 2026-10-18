# HnuSec 2026 秋季培训 · week1 周报

- **ID**：王学强　
- **方向**：Pwn
- **日期**：2026-10-18
- **本周投入**：约15小时 

---

## 一、本周学习内容

### 10/13 · 环境搭建

1. 于清华园镜像上成功安装 Ubuntu 系统于 VMware；
2. 将 pwn 的一些必要工具如 **pwntools**、**pwndbg** 等安装到我的 Ubuntu 系统；
3. 安装 IDA Pro。

**当日练习**：尝试于 IDA 上破解 p0（USEYOURIDA）。熟悉操作中，但并未找到 flag，明日继续努力。

### 10/14 · 第一面 flag + 栈溢出基础

**1. 成功破解昨日未破解的 flag：`flag{how_tu_start_pwn}`**

WP：首先于 IDA 上打开 USEYOURIDA，然后在 main 上找到 `message`，点开后按 Ctrl+F 打开页面搜索，之后按 Shift+F12 打开 Strings 字符串窗口查找 flag。

收获：Shift+F12 打开的 Strings 窗口是什么？—— IDA 会扫描出二进制里所有字符串常量。

**2. 阅读栈溢出文章**（https://hello-ctf.com/hc-pwn/Stack_Overflow/ ），初步了解栈的概念、栈帧和返回地址。

基本栈帧结构：RBP 为栈底寄存器，RSP 为栈顶寄存器，分别记录了栈帧中数据部分的起始和终止地址。函数的临时变量在内存中的位置都是通过这两个寄存器加减偏移确定的。栈底还记录了上一个栈帧的 RBP 的值，以及函数的**返回地址**。

在 C 语言中，函数的临时变量存储于栈上。栈的增长方向是高地址向低地址，栈底在高地址一侧。**控制程序流的本质就是控制 rip 寄存器。**

字符串栈溢出基本原理：字符串可以理解为特殊的数组，所以字符串也有与数组类似的特性（编译和运行时并不会检查字符串的长度）。字符串栈溢出漏洞多数是由 `gets`、`scanf`、`read` 等输入函数以及 `memcpy`、`strcpy` 等 `string.h` 库中的函数没有正常处理字符串长度造成的。

（栈上的东西是一块连着的内存。只要下标写太大，就能越界，修改后面相邻内存里存的别的变量。如果再往后延伸，继续越界，就能一路写到返回地址，也就是栈溢出漏洞。）

### 10/15 · C 语言分支结构 + 保护机制

**1. C 语言 `if / else` 结构和 `switch` 结构**

- 差别：`if / else if` 适合范围判断、复杂条件；`switch` 适合对同一个变量做多值等值判断。
- 注意事项：`switch` 忘写 `break` → 会「穿透」继续执行下一个 case。

**2. 常见保护机制**（https://hello-ctf.com/hc-pwn/Protections/ ）

- Canary（金丝雀）：Linux 下 GCC 编译器提供的一种栈溢出保护机制，得名于煤矿里的「金丝雀预警」。核心原理是在函数的返回地址与局部变量之间插入一个随机的「金丝雀」值。函数入口处，程序将该值存入栈中；函数返回前，会检查该值是否被修改。
- **ASLR**           **PIE**       RELRO等（.md格式贴不了图片详细的在我的word文档上）

### 10/16

1. Linux 基本操作
2. C 语言

🔧 **这一条太空，补两句**：这天具体看了什么/整理了什么？（例如整理前一天的笔记、回顾 checksec 四项保护、看 Linux 教程某几节）如实写即可。

### 10/17 · Linux 基础 + 第二题 door

1. 继续学习 Linux 基础（www.itheima.com ）；
2. 尝试破解第 2 个 flag：door。
   - 第一步：将 door 传到 Linux 系统，在终端执行 `checksec /home/uni/door`，发现有 **Canary、NX、PIE**。
   - 第二步：在 IDA 中打开 door 查看，点击 main 查看主函数，之后 Shift+F12 打开字符串窗口，然后 Ctrl+F 查找 flag。

🔧 **这一步没有成功，原因和正确方向见下方「技术问题 1」**

---

## 二、题目 WP

### 题目 1：USEYOURIDA

- **平台/来源**：群里的文档
- **环境与工具**：IDA
- **解题过程**：
  1. 用 IDA 打开程序，进入 `main` 函数；
  2. 定位到 `message` 相关内容；
  3. 按 `Shift+F12` 打开 Strings 窗口，查看全部字符串常量；
  4. 在 `.rodata` 段中找到明文 flag；
  5. 用 `Ctrl+F` 在字符串列表中搜索，快速定位。
- **关键操作**：`Shift+F12`（Strings 窗口）、`Ctrl+F`（搜索）、`.rodata` 段
- **结果**：`flag{how_tu_start_pwn}`
- **收获**：这题让我知道「flag 有时就明文躺在文件里，先 `strings` 再上 IDA

### 题目 2：door（进行中）

- **环境**：Ubuntu（VMware）+ IDA + checksec
- **保护情况**：Canary  / NX  / PIE 
- **已做的部分**：checksec 确认保护；IDA 打开 main；Shift+F12 查看字符串
- **卡点**：不知如何拿到key

---

## 三、环境与工具

- 虚拟机：VMware + Ubuntu 24.04
- Pwn 工具链：pwntools、pwndbg、checksec
- 逆向：IDA Pro
- 参考文档：Hello-CTF（栈溢出 / 保护机制）

---

## 四、遇到的困难与解决

1. **`apt install` 报 `Unable to locate package`** —— 原因是 apt 索引过期（`Reading package lists... Done` 只是读了旧索引），解决：先 `sudo apt update`。
2. **共享文件夹里的二进制报 `Permission denied`** —— 共享目录没有 Linux 执行位，解决：先 `cp` 到 `~`，再 `chmod +x`。
3. **VMware「客户机隔离」的拖放/复制粘贴勾不上** —— 原因是虚拟机必须**完全关机**才能修改该设置，解决：关机后勾选。
4. **Ubuntu 黑屏** —— 分层判断：自动息屏 / 图形界面崩溃（`Ctrl+Alt+F2` + `systemctl restart gdm3`）/ 完全无响应（强制重启）；后续关闭 3D 加速、改 Xorg、关闭自动息屏。

---

## 五、下周计划

1. 完成 door 的密码定位，拿到 shell 并 `cat flag`；
2. 独立完成一道 ret2text 栈溢出并写 WP；
3. 开始学 ret2libc / ROP 入门；

---

## 六、参考文章

- Hello-CTF · 栈溢出：https://hello-ctf.com/hc-pwn/Stack_Overflow/
- Hello-CTF · 保护机制：https://hello-ctf.com/hc-pwn/Protections/
- 黑马程序员 Linux 教程：www.itheima.com



（学长我是在word文档里写的然后再让AI帮我转换成.md格式的所以可能会有一些AI 味但是这大部分都都是我写的或从网页上copy的）




