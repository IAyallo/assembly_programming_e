# Multiplication Flags

For unsigned `MUL`, only `CF` and `OF` are defined: they are set when the upper half of the product is nonzero, and cleared when it is zero. `SF`, `ZF`, `AF`, and `PF` are undefined. Flag states below are immediately after `MUL`.

| Example | Product | Set flags | Cleared flags | Why |
| --- | --- | --- | --- | --- |
| `mul1.asm`: `25 * 10` | `250` (`AH:AL = 0x00FA`) | None | `CF`, `OF` | The product fits in the low byte, so the upper half (`AH`) is zero. |
| `mul2.asm`: `3000 * 200` | `600000` (`DX:AX = 0x0009:0x27C0`) | `CF`, `OF` | None | The upper half (`DX`) is nonzero, so the product needs more than the low word. |
