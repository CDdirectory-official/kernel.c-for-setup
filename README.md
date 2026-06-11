# README And Read The Explained!

A hobbyist project built from scratch to eliminate the overhead of traditional, bloated kernels. No Linux base, no GNU utilities—just pure, raw communication with the hardware.

## 🛠️ Architecture Overview
* **Bootloader:** Handwritten x86 Assembly (NASM) switching from 16-bit Real Mode to Protected Mode.
* **Kernel Core:** Written in freestanding C, mapping directly to VGA Video Memory (`0xB8000`).
* **Environment:** Designed specifically for virtualization using **QEMU** for safe, hyper-fast debugging without risking real hardware.

## ⚡ Quick Start (No-Brainer Boot)
If you want to compile and run this kernel inside QEMU, run these commands in your terminal:

```bash
# 1. Assemble the boot entry
nasm -f elf32 kernel_entry.asm -o k_entry.o

# 2. Compile the C kernel without standard libraries
gcc -m32 -c kernel.c -o kernel.o -ffreestanding

# 3. Link them into a raw binary
ld -m elf_i386 -o kernel.bin -Ttext 0x1000 k_entry.o kernel.o --oformat binary

# 4. Fire up the Virtual Machine!
qemu-system-x86_64 -drive format=raw,file=kernel.bin
