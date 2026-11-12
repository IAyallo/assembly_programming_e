# 01.asm — Basic push and pop (64-bit)

This program demonstrates that the stack is last-in, first-out (LIFO): the last value pushed is the first one popped.

## Execution and stack flow

Let `S` be the value of `RSP` on entry to `_start`. Linux supplies the initial stack; the program does not set its starting address.

1. `mov rax, 100` and `mov rbx, 200` put the values in registers. These instructions do not use the stack, so `RSP` remains `S`.
2. `push rax` moves `RSP` down by 8 bytes to `S - 8` and stores `100` there.
3. `push rbx` moves it down another 8 bytes to `S - 16` and stores `200` at the new top. The top of the stack is now `200`, above the earlier `100`.
4. `nop` does nothing. `RSP` remains `S - 16`.
5. `pop rcx` reads the top value (`200`) into `RCX`, then moves `RSP` up 8 bytes to `S - 8`.
6. `pop rdx` reads the remaining value (`100`) into `RDX`, then restores `RSP` to `S`.
7. The second `nop` does nothing. The stack is back to its entry position; there are no values left from these pushes.
8. The program loads Linux's 64-bit `exit` syscall number (`60`) into `RAX`, sets its status argument `RDI` to zero, and executes `syscall`. This terminates the process; the syscall does not push a return address onto this user stack.

## Run observation

The program performs no write or print operation, so it exits silently. Its exit syscall requests status `0`. The values are moved between registers and the stack but are never displayed.
