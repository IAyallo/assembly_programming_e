# Division Flags

For unsigned `DIV`, the status flags `CF`, `OF`, `SF`, `ZF`, `AF`, and `PF` are undefined. The instruction does not provide reliable set or cleared states for them, so inspect the quotient and remainder registers instead. These results are immediately after `DIV`.

| Example | Quotient | Remainder | Flags |
| --- | --- | --- | --- |
| `div1.asm`: `100 / 7` | `14` in `AL` | `2` in `AH` | Undefined; `DIV` does not define arithmetic status flags, so any observed `CF`, `OF`, `SF`, `ZF`, `AF`, or `PF` value is not a meaningful consequence of this division. The quotient and remainder satisfy `14 * 7 + 2 = 100`. |
| `div2.asm`: `50000 / 300` | `166` in `AX` | `200` in `DX` | Undefined; `DIV` does not define arithmetic status flags, so any observed `CF`, `OF`, `SF`, `ZF`, `AF`, or `PF` value is not a meaningful consequence of this division. The quotient and remainder satisfy `166 * 300 + 200 = 50000`. |
