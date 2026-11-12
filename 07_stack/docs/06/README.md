# 06.asm — Push and pop 32-bit values

This 32-bit example mirrors the two-value push/pop demonstration, using 4-byte stack entries and `ESP`.

## Execution and stack flow

Let `S` be `ESP` on entry to `_start`; Linux supplies the initial stack address.

1. `mov eax, 0x11223344` and `mov ebx, 0x1234` initialize registers without changing `ESP`.
2. `push eax` decreases `ESP` by 4 bytes to `S - 4` and stores `0x11223344`.
3. `push ebx` decreases `ESP` by another 4 bytes to `S - 8` and stores `0x1234` at the top.
4. `nop` has no effect on the stack.
5. `pop ebx` retrieves the most recently pushed value (`0x1234`) and increases `ESP` to `S - 4`.
6. `pop eax` retrieves the earlier value (`0x11223344`) and restores `ESP` to `S`. The original register/value pairing is recovered.
7. The program loads exit syscall number `1` into `EAX`, sets `EBX` to exit status `0`, and invokes `int 0x80`. The process exits; the syscall does not push application data onto the stack.

The stack grows toward lower addresses when values are pushed and toward higher addresses when popped. Here both pushes are balanced by two pops, so `ESP` is back at its entry position before the exit syscall.

## Run observation

There is no write/print syscall, so the program exits without output. The exit request sets status `0`; the register values are not printed.
