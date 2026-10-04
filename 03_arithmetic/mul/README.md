# Multiplication Flags

For unsigned `MUL`, only `CF` and `OF` are defined. Both are set when the upper half of the product is nonzero, because the product does not fit in the low half; both clear when the upper half is zero, because it fits. `SF`, `ZF`, `AF`, and `PF` are undefined, so their resulting values cannot be explained or relied on. Flag states below are immediately after `MUL`.

| Example | Product | Set flags | Cleared flags | Why |
| --- | --- | --- | --- | --- |
| `mul1.asm`: `25 * 10` | `250` (`AH:AL = 0x00FA`) | None | `CF`, `OF` | `AH` is zero, so the 16-bit product fits in the low byte `AL`; therefore `CF` and `OF` both clear. The other arithmetic flags are undefined by `MUL`. |
| `mul2.asm`: `3000 * 200` | `600000` (`DX:AX = 0x0009:0x27C0`) | `CF`, `OF` | None | `DX` is nonzero, so the 32-bit product does not fit in the low word `AX`; therefore `CF` and `OF` both set. The other arithmetic flags are undefined by `MUL`. |
