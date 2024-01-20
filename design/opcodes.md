# Instruction Opcodes

## Type Sizes and Definitions

Whenever the "type" of a value is referenced, it is important to note that they follow the [U language primitive type specification](https://github.com/398utubzyt/ulang-docs/blob/main/design/core/types.md).

For reference, the list of used types and their sizes are below:

| Name | Byte Size | Notes
|-----|-----|-----
| `i8` | 1 | Signed 8-bit integer
| `i16` | 2 | Signed 16-bit integer
| `i32` | 4 | Signed 32-bit integer
| `i64` | 8 | Signed 64-bit integer
| `i128` | 16 | Signed 128-bit integer
| `isize` | N/A | Signed pointer-sized integer
| `u8` | 1 | Unsigned 8-bit integer
| `u16` | 2 | Unsigned 16-bit integer
| `u32` | 4 | Unsigned 32-bit integer
| `u64` | 8 | Unsigned 64-bit integer
| `u128` | 16 | Unsigned 128-bit integer
| `usize` | N/A | Unsigned pointer-sized integer
| `f16` | 2 | 16-bit floating point number
| `f32` | 4 | 32-bit floating point number
| `f64` | 8 | 64-bit floating point number
| `f128` | 16 | 128-bit floating point number
| `bool` | 1 | Boolean

## Opcode List

Below is a table of valid opcodes and their descriptions. All opcodes are 16 bits (2 bytes) in size unless otherwise specified.

| Opcode | Instruction | Description |
|----|----|----|
| `0000` | `nop` | No operation |
| `0001` | `break` | Debugger breakpoint |
| `005A` | `call <u32:id>` | Calls the specified function |
| `005B` | `ret` | Returns from the current function |
