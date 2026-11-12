# 02.asm — Stack frame with local variables (64-bit)

This program calls `work`, saves the frame pointer, reserves 16 bytes for local values, reads those values back, and restores the frame before returning.

## Execution and stack flow

Let `S` be `RSP` at `_start`. The initial pointer is supplied by Linux and is not a fixed address in the source.

1. `call work` first pushes the address of the instruction after the call onto the stack. `RSP` becomes `S - 8`; execution branches to `work`.
2. `push rbp` saves the incoming `RBP` value at the new top and changes `RSP` to `S - 16`.
3. `mov rbp, rsp` establishes the frame base at `S - 16`. `RBP` now stays fixed while `RSP` is moved to allocate and release locals.
4. `sub rsp, 16` reserves 16 bytes, moving `RSP` to `S - 32`. No values are pushed by this subtraction; it simply makes space.
5. The stores write `100` to `[rbp - 8]` (address `S - 24`) and `200` to `[rbp - 16]` (address `S - 32`). These are 8-byte local slots. They are accessed by frame-relative memory addresses, not by `pop`.
6. The loads copy the local `100` into `RAX` and `200` into `RBX`. They do not change either stack pointer. `nop` also leaves the stack unchanged.
7. `mov rsp, rbp` discards the 16-byte local area by restoring `RSP` from `S - 32` to `S - 16`.
8. `pop rbp` restores the saved incoming frame pointer and moves `RSP` to `S - 8`. The call's return address is now at the top.
9. `ret` pops that return address into the instruction pointer and moves `RSP` back to `S`, returning to `_start`.
10. `_start` exits with syscall 60 and status zero. The exit syscall ends the process rather than returning through the stack.

The call return address and saved `RBP` are separate stack entries from the local-variable area. Restoring `RSP` before `pop rbp` ensures `ret` finds the call's return address rather than local data.

## Run observation

There is no output instruction, so the program completes silently. The exit syscall requests status `0`; the loaded local values are not printed.
