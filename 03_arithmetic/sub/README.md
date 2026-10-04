# Subtraction Flags

Flag states below are those immediately after the subtraction instruction. The later `xor ebx, ebx` used for process exit changes some flags.

| Example | Result | Set flags | Cleared flags | Why |
| --- | --- | --- | --- | --- |
| `sub1.asm`: `50 - 80` in `AL` | `226` (`0xE2`, signed `-30`) | `CF`, `SF`, `PF` | `OF`, `ZF`, `AF` | `CF` sets because unsigned 50 is less than 80, so subtraction borrows. `OF` clears because signed `-30` fits in 8 bits. `SF` sets because bit 7 of `0xE2` is 1; `ZF` clears because the result is nonzero. `AF` clears because low-nibble `2 - 0` needs no borrow across bit 3. `PF` sets because `0xE2` has four 1-bits (even parity). |
| `sub2.asm`: `1000 - 2000` in `AX` | `64536` (`0xFC18`, signed `-1000`) | `CF`, `SF`, `PF` | `OF`, `ZF`, `AF` | `CF` sets because unsigned 1000 is less than 2000, so subtraction borrows. `OF` clears because signed `-1000` fits in 16 bits. `SF` sets because bit 15 of `0xFC18` is 1; `ZF` clears because the result is nonzero. `AF` clears because low-nibble `8 - 0` needs no borrow across bit 3. `PF` sets because the low byte `0x18` has two 1-bits (even parity). |
