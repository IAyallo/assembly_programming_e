# Subtraction Flags

Flag states below are those immediately after the subtraction instruction. The later `xor ebx, ebx` used for process exit changes some flags.

| Example | Result | Set flags | Cleared flags | Why |
| --- | --- | --- | --- | --- |
| `sub1.asm`: `50 - 80` in `AL` | `226` (`0xE2`, signed `-30`) | `CF`, `SF`, `PF` | `OF`, `ZF`, `AF` | Unsigned subtraction borrows, so `CF` is set. The result's high bit is set, it is nonzero, and `0xE2` has even parity. The signed result fits in 8 bits, and the low-nibble subtraction needs no borrow. |
| `sub2.asm`: `1000 - 2000` in `AX` | `64536` (`0xFC18`, signed `-1000`) | `CF`, `SF`, `PF` | `OF`, `ZF`, `AF` | Unsigned subtraction borrows, so `CF` is set. The 16-bit result has its high bit set, is nonzero, and its low byte has even parity. The signed result fits in 16 bits, and the low-nibble subtraction needs no borrow. |
