# 03.asm — Call and return with a register result (64-bit)

This example shows that a procedure can use a value in a register and return it without pushing that value onto the stack.

## Execution and stack flow

Let `S` be `RSP` at `_start`; its absolute value is supplied by Linux.

1. `mov rax, 10` places the input value in `RAX`. `RSP` remains `S`.
2. `call calculate` pushes the return address—the location of `after_call`—onto the stack. `RSP` decreases by 8 bytes to `S - 8`, then execution transfers to `calculate`.
3. `add rax, 5` changes the register value from `10` to `15`. It does not access the stack, so `RSP` stays `S - 8`.
4. `ret` pops the saved return address into the instruction pointer, increments `RSP` by 8 bytes back to `S`, and resumes at `after_call`. `RAX` still contains `15`; the result is returned in a register, not on the stack.
5. `nop` leaves registers and stack unchanged. The comment notes the expected `RAX` value, but the program does not display it.
6. `_start` then loads syscall number `60` into `RAX` and status `0` into `RDI`, and executes `syscall` to exit.

The only stack entry created by this program is the temporary return address from `call`. A `call`/`ret` pair balances it, leaving the stack pointer at its entry value before exit.

## Run observation

The program prints nothing. Although `calculate` produces `15` in `RAX`, `_start` immediately overwrites `RAX` with the exit syscall number; the process exits with the requested status `0`.
