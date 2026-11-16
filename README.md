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

The purpose of this section is to get rust to compile a **bare metal** application. A bare metal application is one that requires no OS to run. The application has direct access to the hardware resources with no kernel. However, this means we lose the standard library and everything built on these abstractions. Thats okay because we need our kernel to be a bare-metal application. 