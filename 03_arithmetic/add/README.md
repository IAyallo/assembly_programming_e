# Addition Flags

Flag states below are those immediately after the arithmetic instruction. The later `xor ebx, ebx` used for process exit changes some flags.

| Example | Result | Set flags | Cleared flags | Why |
| --- | --- | --- | --- | --- |
| `add1.asm`: `120 + 10` in `AL` | `130` (`0x82`) | `OF`, `SF`, `AF`, `PF` | `CF`, `ZF` | There is no unsigned carry, but the signed sum exceeds the 8-bit signed range. The result's high bit is set, it is nonzero, the low-nibble addition carries, and `0x82` has even parity. |
| `add2.asm`: `32000 + 500` in `AX` | `32500` (`0x7EF4`) | None | `CF`, `OF`, `SF`, `ZF`, `AF`, `PF` | The sum fits both unsigned and signed 16-bit ranges, is positive and nonzero, has no low-nibble carry, and its low byte has odd parity. |
| `add3.asm`: `0xFFFF + 1` with `ADD` | `0` (`0x0000`) | `CF`, `PF`, `AF`, `ZF` | `OF`, `SF` | The addition carries out of the 16-bit word and low nibble, producing zero. Zero has even parity; adding signed `-1` and `1` does not overflow. |
| `add3.asm`: following `ADC AX, 0` | `1` (`0x0001`) | None | `CF`, `OF`, `SF`, `ZF`, `AF`, `PF` | `ADC` consumes the carry from `ADD`; the final result has no outgoing carry, is positive and nonzero, has no low-nibble carry, and has odd parity. |
