# 04.asm — 32-bit stack frame with local variables

This is the 32-bit counterpart to the stack-frame example: `work` saves `EBP`, reserves space for two local values, reads them, then restores the frame and returns.

## Execution and stack flow

Let `S` be `ESP` on entry to `_start`. The actual starting address comes from Linux; only the relative changes below are determined by the program. In 32-bit mode, each pushed value and return address occupies 4 bytes.

1. `call work` pushes the return address (the instruction after `call`) and sets `ESP` to `S - 4`, then branches to `work`.
2. `push ebp` saves the incoming `EBP` value and decreases `ESP` to `S - 8`.
3. `mov ebp, esp` sets the frame base to `S - 8`.
4. `sub esp, 8` reserves 8 bytes for two 4-byte local slots, moving `ESP` to `S - 16`.
5. `mov dword [ebp - 4], 100` stores `100` at address `S - 12`; `mov dword [ebp - 8], 200` stores `200` at `S - 16`. These locals are addressed relative to `EBP`; they are not pushed or popped individually.
6. The two loads retrieve those locals into `EAX` (`100`) and `EBX` (`200`). They do not change `ESP`. `nop` has no effect.
7. `mov esp, ebp` releases the local area, restoring `ESP` from `S - 16` to `S - 8`.
8. `pop ebp` restores the saved frame pointer and moves `ESP` to `S - 4`, where the call return address is now on top.
9. `ret` pops the return address, moves `ESP` back to `S`, and resumes in `_start`.
10. `_start` uses the 32-bit Linux exit syscall (`EAX = 1`) with `EBX = 0`, invoked by `int 0x80`. The process terminates with status zero.

The saved `EBP` and return address are control/frame data on the stack; the local integers occupy the separately reserved frame area. The explicit frame cleanup ensures `ret` reads the correct address.

## Run observation

The program has no output operation and therefore exits silently. Its `int 0x80` exit request specifies status `0`; the local values are not shown.
