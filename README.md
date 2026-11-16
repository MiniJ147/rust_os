# RustOS

Simple operating system built in Rust with the following resources:
1. [osdev.org](https://wiki.osdev.org/Introduction)
2. [Writing an OS in Rust by Philipp Oppermann](https://os.phil-opp.com/)

# Dev Log
A Collection of notes and changes made throughout this project.

## 11/16/26:
Followed: [A Freestanding Rust Binary](https://os.phil-opp.com/freestanding-rust-binary/)
1. Removed std 
2. Provided Panic Implementation
3. Disabled Unwinding
4. Removed Main and Provide C _start func
5. Removed linking errors

to build:
```bash
rustup target add thumbv7em-none-eabihf # if you don't have it installed

cargo build --target thumbv7em-none-eabihf  
```

The purpose of this section is to get rust to compile a **bare metal** application. 
A bare metal application is one that requires no OS to run. 
The application has direct access to the hardware resources with no kernel. 
However, this means we lose the standard library and everything built on these abstractions. Thats okay because we need our kernel to be a bare-metal application. 

## Part 2
Followed: [A Minimal Rust Kernel](https://os.phil-opp.com/minimal-rust-kernel/)
yada...

## The boot process:  
When you turn on your computer, it begins executing firmware code that is stored in the motherboards ROM.
This code performs a **power-on self tests (POST)**, to set the initial state of the device and detect if any hardware components are non-functional. It detects available RAM, and rpe-initializes the CPU and hardware. 
Afterwards, it looks for a bootable disk and starts booting the operating system kernel.

On x86 there are two firmware standards:
1. Basic Input/Output System (BIOS)
2. Unified Extensible Firmware Interface (UEFI).

```
Personal Note: (not from blog)
The purpose of these 2 firmwares is to act as the low-level bridge between a computers physical hardware and the operating system during the pre-boot startup phase.

When you turn on your computer, the CPU startrs in a blank state with no drivers or operating system loaded in RAM. 

The motherboard firmware steps in to:
1. Perform a POST: check vital hardware is present and working 
2. Initialize Hardware: wake up and configure system components like input devices, graphics output, etc..
3. Provide System Data & Background tasks
4. Hand Off to the OS: locate a bootable storage drive, read the bootloader, and launch the operating system.

This bootable firmware is the classic Chicken and Egg problem. We need the firmware to get some jumping off point.

UEFI replaced BIOS because legacy BIOS could no longer handle modern computer hardware, storage sizes, and security demands.
```

The BIOS after running its tests on the hardware looks for *bootable disks*. 
If it finds one the control is transferred to its *bootloader*, which is a 512-byte protion of executable code stored at the disk's beginning.
Most bootloaders are large than 512 bytes, so bootloaders are commonly split into a small stage, which fits into 512 bytes, and a second stage, which is loaded by the first stage.

The *bootloader* has to determine the location of the *kernel image* on the disk and load it into memory.

It also needs to switch the CPU into 64-bit long moe, where 64-bit registers and the complete main memory is available.

Its third job is to query certain information from the BOIS and pass it to the OS kernel.

Writing a bootloader is a bit cumbersome and requires assembly language and a lot of magic values. To bypass this you can use a tool named **bootimage** that automatically prepends a bootloader to your kernel. For now I will skip building a bootloader and will use the tool instead.

## Summary
I think I am happy with todays progress. 

I would like to go back and learn a little fundamental concepts. I feel like I'm jumping a little too fast?

Other than that this is the farthest I got with an OS project. Still confused on a lot of things, but just going to keep pushing and see. I think I fall into the trap of trying to understand everything all at once, instead of just tackling it bit by bit. 