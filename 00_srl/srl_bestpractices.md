# SRL Best Practices

## Avoid using `float` and `double` datatypes.

The SH-2 cpu's on the sega saturn does not have floating point hardware, therefore floating point operations are more demanding on the CPU.

Therefore for operations on ($\\mathbb{R}$) numbers, it is recommended to use only fixed point math.
For this SRL implements the [`Fxp` datatype](https://srl.reye.me/classSRL_1_1Math_1_1Types_1_1Fxp.html) that its a 16.16 fixed point type implementation.

> [!CAUTION]
> Avoid runtime conversion to and from `float` or `double` in production code. Doing so is detrimental to performance !


