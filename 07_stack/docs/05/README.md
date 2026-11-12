# 05.asm — Push and pop 64-bit values

This program pushes two different values and pops them into the opposite registers, demonstrating both LIFO order and preservation of the full 64-bit value.

## Execution and stack flow

Let `S` be the initial `RSP` at `_start`; Linux provides this starting pointer.

1. `mov rax, 0x1122334455667788` and `mov rbx, 0x1234` initialize registers without touching the stack. `RSP` remains `S`.
2. `push rax` decreases `RSP` by 8 bytes to `S - 8` and stores `0x1122334455667788`.
3. `push rbx` decreases it another 8 bytes to `S - 16` and stores `0x1234` at the top. The newest value is now on top of the earlier 64-bit value.
4. `nop` changes nothing.
5. `pop rbx` removes the top value `0x1234` into `RBX` and increases `RSP` by 8 bytes to `S - 8`.
6. `pop rax` removes `0x1122334455667788` into `RAX` and restores `RSP` to `S`. Both registers now hold their original values.
7. The code sets `RAX` to syscall number `60`, sets `RDI` to exit status `0`, and executes `syscall`. The exit operation terminates the process; it does not add a user-stack return address.

Each 64-bit push/pop changes `RSP` by 8 bytes. Because the pops are in reverse order from the pushes, the register/value pairing is restored.

## Run observation

The executable produces no printed output. It requests a clean exit with status `0`; the restored register values are not displayed.
