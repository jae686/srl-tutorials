# SRL Best Practices

## Avoid using `float` and `double` datatypes.

The SH-2 cpu's on the sega saturn does not have floating point hardware, therefore floating point operations are more demanding on the CPU.

For real numbers ($\\mathbb{R}$) its is recommended to use the `Fxp` datatype.