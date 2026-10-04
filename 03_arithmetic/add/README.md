# Addition Flags

Flag states below are those immediately after the arithmetic instruction. The later `xor ebx, ebx` used for process exit changes some flags.

| Example | Result | Set flags | Cleared flags | Why |
| --- | --- | --- | --- | --- |
| `add1.asm`: `120 + 10` in `AL` | `130` (`0x82`) | `OF`, `SF`, `AF`, `PF` | `CF`, `ZF` | `CF` clears because the 8-bit sum does not carry past bit 7. `OF` sets because signed `120 + 10` exceeds `127`. `SF` sets because bit 7 of `0x82` is 1; `ZF` clears because the result is nonzero. `AF` sets because `0x8 + 0xA` carries from bit 3 to bit 4. `PF` sets because the low byte `0x82` contains two 1-bits (even parity). |
| `add2.asm`: `32000 + 500` in `AX` | `32500` (`0x7EF4`) | None | `CF`, `OF`, `SF`, `ZF`, `AF`, `PF` | `CF` clears because there is no carry past bit 15. `OF` clears because the positive signed sum fits in a 16-bit signed value. `SF` clears because bit 15 is 0; `ZF` clears because the result is nonzero. `AF` clears because the low-nibble addition `0 + 4` has no carry. `PF` clears because the low byte `0xF4` contains five 1-bits (odd parity). |
| `add3.asm`: `0xFFFF + 1` with `ADD` | `0` (`0x0000`) | `CF`, `PF`, `AF`, `ZF` | `OF`, `SF` | `CF` sets because the sum carries past bit 15. `OF` clears because signed `-1 + 1` is representable. `SF` clears because bit 15 of zero is 0; `ZF` sets because the result is zero. `AF` sets because `0xF + 1` carries from bit 3 to bit 4. `PF` sets because the low byte `0x00` has zero 1-bits (even parity). |
| `add3.asm`: following `ADC AX, 0` | `1` (`0x0001`) | None | `CF`, `OF`, `SF`, `ZF`, `AF`, `PF` | `ADC` adds the carry from the preceding `ADD`, so `0 + 0 + 1 = 1`. `CF` clears because there is no carry past bit 15; `OF` clears because 1 is in signed range. `SF` clears because bit 15 is 0; `ZF` clears because the result is nonzero. `AF` clears because the low-nibble sum is 1 with no carry from bit 3. `PF` clears because `0x01` has one 1-bit (odd parity). |
