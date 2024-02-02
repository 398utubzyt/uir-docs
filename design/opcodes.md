# Instruction Opcodes

## Type Sizes and Definitions

The [Opcode List](#opcode-list) uses type names to indicate usage and size of a particular set of data. Below is a table for reference:

| Name | Byte Size | Notes |
|-----|-----|-----|
| `i8` | 1 | Signed 8-bit integer |
| `i16` | 2 | Signed 16-bit integer |
| `i32` | 4 | Signed 32-bit integer |
| `i64` | 8 | Signed 64-bit integer |
| `i128` | 16 | Signed 128-bit integer |
| `u8` | 1 | Unsigned 8-bit integer |
| `u16` | 2 | Unsigned 16-bit integer |
| `u32` | 4 | Unsigned 32-bit integer |
| `u64` | 8 | Unsigned 64-bit integer |
| `u128` | 16 | Unsigned 128-bit integer |
| `f16` | 2 | 16-bit floating point number |
| `f32` | 4 | 32-bit floating point number |
| `f64` | 8 | 64-bit floating point number |
| `f128` | 16 | 128-bit floating point number |
| `bool` | 1 | Boolean |

Some types specified here are not defined in the ULang specification. Below is a table defining these types using the types above:

| Name | Format | Notes |
|-----|-----|-----|
| `imm` | `<u8:size, size:value>` | An immediate (hardcoded, constant) value. `size` is in bytes. |

## Conversion Values

### Floating-point

Below is a table containing values corresponding to a specific floating-point precision.

| Name | Value |
|-----|-----|
| `f16` | `1` |
| `f32` | `2` |
| `f64` | `3` |
| `f128` | `4` |

### Integer

Below is a table containing values corresponding to a specific integer precision.

| Name | Value |
|-----|-----|
| `i8` | `0` |
| `i16` | `1` |
| `i32` | `2` |
| `i64` | `3` |
| `i128` | `4` |

## Avaliable Registers

UIR exposes "virtual registers", meaning that even though these registers may not physically exist,
they represent real registers and provide an easy way of translating UIR to real machine code. While
most registers do not have explicit purposes, the table below is a :

| ID(s) | Name | Description |
|----|----|----|
| `00` | Zero | This register will always return `0` and any value written to it will be discarded. |
| `01` | Stack Pointer |  |
| `09-7F` | General Purpose Integer |  |
| `80-FF` | General Purpose Floating-point |  |

> Note: All of the above registers (with the exception of Zero/`00`) can be used as general purpose registers.

## Opcode List

Below is a table of valid opcodes and their descriptions. All opcodes are 8 bits (1 byte) in size unless otherwise specified.

| Opcode | Instruction | Arguments | Description |
|----|----|----|----|
| `00` | `nop` | `<>` | No operation. |
| `01` | `break` | `<>` | Debugger breakpoint. |
| `02` | `ld8` | `<u8:dest, u8:src>` | Loads an 8-bit value from the address in `src` into a register `dest`. |
| `03` | `ld16` | `<u8:dest, u8:src>` | Loads a 16-bit value from the address in `src` into a register `dest`. |
| `04` | `ld32` | `<u8:dest, u8:src>` | Loads a 32-bit value from the address in `src` into a register `dest`. |
| `05` | `ld64` | `<u8:dest, u8:src>` | Loads a 64-bit value from the address in `src` into a register `dest`. |
| `06` | `ld128` | `<u8:dest, u8:src>` | Loads a 128-bit value from the address in `src` into a register `dest`. |
| `08` | `ldptr` | `<u8:dest, u8:src, u8:size>` | Loads a `size`-sized value from the address in `src` into a register `dest`. `size` is in bytes. |
| `09` | `st8` | `<u8:dest, u8:src>` | Stores an 8-bit value from the register `src` into the address in `dest`. |
| `0A` | `st16` | `<u8:dest, u8:src>` | Stores a 16-bit value from the register `src` into the address in `dest`. |
| `0B` | `st32` | `<u8:dest, u8:src>` | Stores a 32-bit value from the register `src` into the address in `dest`. |
| `0C` | `st64` | `<u8:dest, u8:src>` | Stores a 64-bit value from the register `src` into the address in `dest`. |
| `0D` | `st128` | `<u8:dest, u8:src>` | Stores a 128-bit value from the register `src` into the address in `dest`. |
| `0F` | `stptr` | `<u8:dest, u8:src, u8:size>` | Stores a `size`-bit value from the register `src` into the address in `dest`. `size` is in bytes. |
| `10` | `add` | `<u8:dest, u8:op1, u8:op2>` | Adds the values stored in registers `op1` and `op2` and stores the result in `dest`. |
| `11` | `sub` | `<u8:dest, u8:op1, u8:op2>` | Subtracts the values stored in registers `op1` from `op2` and stores the result in `dest`. |
| `12` | `mul` | `<u8:dest, u8:op1, u8:op2>` | Multiplies the values stored in registers `op1` and `op2` and stores the result in `dest`. |
| `13` | `div` | `<u8:dest, u8:op1, u8:op2>` | Divides the values stored in registers `op1` by `op2` and stores the result in `dest`. |
| `14` | `divu` | `<u8:dest, u8:op1, u8:op2>` | Divides the unsigned values stored in registers `op1` by `op2` and stores the result in `dest`. |
| `15` | `rem` | `<u8:dest, u8:op1, u8:op2>` | Gets the remainder of the division of the values stored in registers `op1` by `op2` and stores the result in `dest`. |
| `16` | `remu` | `<u8:dest, u8:op1, u8:op2>` | Gets the remainder of the division of the unsigned values stored in registers `op1` by `op2` and stores the result in `dest`. |
| `17` | `shl` | `<u8:dest, u8:op1, u8:op2>` | Logically bitshifts the value in register `op1` to the left by the value in register `op2` and stores the result in `dest`. |
| `18` | `shla` | `<u8:dest, u8:op1, u8:op2>` | Arithmetically bitshifts the value in register `op1` to the left by the value in register `op2` and stores the result in `dest`. |
| `19` | `shr` | `<u8:dest, u8:op1, u8:op2>` | Logically bitshifts the value in register `op1` to the right by the value in register `op2` and stores the result in `dest`. |
| `1A` | `shra` | `<u8:dest, u8:op1, u8:op2>` | Arithmetically bitshifts the value in register `op1` to the right by the value in register `op2` and stores the result in `dest`. |
| `1B` | `and` | `<u8:dest, u8:op1, u8:op2>` | Logically ANDs the value in register `op1` with the value in register `op2` and stores the result in `dest`. |
| `1C` | `or` | `<u8:dest, u8:op1, u8:op2>` | Logically ORs the value in register `op1` with the value in register `op2` and stores the result in `dest`. |
| `1D` | `xor` | `<u8:dest, u8:op1, u8:op2>` | Logically XORs the value in register `op1` with the value in register `op2` and stores the result in `dest`. |
| `20` | `addi` | `<u8:dest, u8:op1, imm:value>` | Adds the value stored in register `op1` and `value`. |
| `21` | `subi` | `<u8:dest, u8:op1, imm:value>` | Subtracts the value stored in register `op1` from `value`. |
| `22` | `muli` | `<u8:dest, u8:op1, imm:value>` | Multiplies the value stored in register `op1` by `value`. |
| `23` | `divi` | `<u8:dest, u8:op1, imm:value>` | Divides the value stored in register `op1` by `value`. |
| `24` | `shli` | `<u8:dest, u8:op1, imm:value>` | Logically bitshifts the value in register `op1` to the left by `value` and stores the result in `dest`. |
| `25` | `shlai` | `<u8:dest, u8:op1, imm:value>` | Arithmetically bitshifts the value in register `op1` to the left by `value` and stores the result in `dest`. |
| `26` | `shri` | `<u8:dest, u8:op1, imm:value>` | Logically bitshifts the value in register `op1` to the right by `value` and stores the result in `dest`. |
| `27` | `shrai` | `<u8:dest, u8:op1, imm:value>` | Arithmetically bitshifts the value in register `op1` to the right by `value` and stores the result in `dest`. |
| `28` | `andi` | `<u8:dest, u8:op1, imm:value>` | Logically ANDs the value in register `op1` with `value` and stores the result in `dest`. |
| `29` | `ori` | `<u8:dest, u8:op1, imm:value>` | Logically ORs the value in register `op1` with `value` and stores the result in `dest`. |
| `2A` | `xori` | `<u8:dest, u8:op1, imm:value>` | Logically XORs the value in register `op1` with `value` and stores the result in `dest`. |
| `30` | `jof` | `<i16:offset>` | Jumps to an instruction by `offset` using the current instruction as the origin. |
| `31` | `jofl` | `<u8:label>` | Jumps to the specified label. |
| `32` | `beq` | `<u8:op1, u8:op2>` | Branch if the values in registers `op1` and `op2` are equal. |
| `33` | `bne` | `<u8:op1, u8:op2>` | Branch if the values in registers `op1` and `op2` are not equal. |
| `34` | `bgt` | `<u8:op1, u8:op2>` | Branch if the value in register `op1` is greater than the value in register `op2`. |
| `35` | `blt` | `<u8:op1, u8:op2>` | Branch if the value in register `op1` is less than the value in register `op2`. |
| `36` | `bge` | `<u8:op1, u8:op2>` | Branch if the value in register `op1` is greater than or equal to the value in register `op2`. |
| `37` | `ble` | `<u8:op1, u8:op2>` | Branch if the value in register `op1` is less than or equal to the value in register `op2`. |
| `38` | `bgi` | `<u8:op1, imm:value>` | Branch if the value in register `op1` is greater than `value`. |
| `39` | `bli` | `<u8:op1, imm:value>` | Branch if the value in register `op1` is less than `value`. |
| `40` | `fadd` | `<u8:dest, u8:op1, u8:op2>` | Adds the floating-point values stored in registers `op1` and `op2` and stores the result in `dest`. |
| `41` | `fsub` | `<u8:dest, u8:op1, u8:op2>` | Subtracts the floating-point values stored in registers `op1` from `op2` and stores the result in `dest`. |
| `42` | `fmul` | `<u8:dest, u8:op1, u8:op2>` | Multiplies the floating-point values stored in registers `op1` and `op2` and stores the result in `dest`. |
| `43` | `fdiv` | `<u8:dest, u8:op1, u8:op2>` | Divides the floating-point values stored in registers `op1` by `op2` and stores the result in `dest`. |
| `44` | `frem` | `<u8:dest, u8:op1, u8:op2>` | Gets the remainder if |
| `45` | `fcvt` | `<u8:dest, u8:target, u8:precision>` | Converts the floating-point value in `from` to the target `precision` and stores the result in `dest`. |
| `46` | `ifcvt` | `<u8:dest, u8:target, u8:precision>` | Converts the integer value in `from` to the target floating-point `precision` and stores the result in `dest`. |
| `47` | `ficvt` | `<u8:dest, u8:target, u8:precision>` | Converts the floating-point value in `from` to the target integer `precision` and stores the result in `dest`. |
| `48` | `fsqrt` | `<u8:dest, u8:operand>` | Calculates the square root of the value in register `operand` and stores the result in `dest`. |
| `49` | `fmin` | `<u8:dest, u8:op1, u8:op2>` | Gets the minimum value between the registers `op1` and `op2` and stores the result in `dest`. |
| `4A` | `fmax` | `<u8:dest, u8:op1, u8:op2>` | Gets the maximum value between the registers `op1` and `op2` and stores the result in `dest`. |
| `4B` | `fsin` | `<u8:dest, u8:operand>` | Calculates the sine of the value in register `operand` in radians and stores the result in `dest`. |
| `4C` | `fcos` | `<u8:dest, u8:operand>` | Calculates the cosine of the value in register `operand` in radians and stores the result in `dest`. |
| `4D` | `ftan` | `<u8:dest, u8:operand>` | Calculates the tangent of the value in register `operand` in radians and stores the result in `dest`. |
| `4E` | `fasin` | `<u8:dest, u8:operand>` | Calculates the arcsine of the value in register `operand` in radians and stores the result in `dest`. |
| `4F` | `facos` | `<u8:dest, u8:operand>` | Calculates the arccosine of the value in register `operand` in radians and stores the result in `dest`. |
| `50` | `fatan` | `<u8:dest, u8:operand>` | Calculates the arctangent of the value in register `operand` in radians and stores the result in `dest`. |
| `51` | `fsinh` | `<u8:dest, u8:operand>` | Calculates the hyperbolic sine of the value in register `operand` in radians and stores the result in `dest`. |
| `52` | `fcosh` | `<u8:dest, u8:operand>` | Calculates the hyperbolic cosine of the value in register `operand` in radians and stores the result in `dest`. |
| `53` | `ftanh` | `<u8:dest, u8:operand>` | Calculates the hyperbolic tangent of the value in register `operand` in radians and stores the result in `dest`. |
| `54` | `fasinh` | `<u8:dest, u8:operand>` | Calculates the hyperbolic arcsine of the value in register `operand` in radians and stores the result in `dest`. |
| `55` | `facosh` | `<u8:dest, u8:operand>` | Calculates the hyperbolic arccosine of the value in register `operand` in radians and stores the result in `dest`. |
| `56` | `fatanh` | `<u8:dest, u8:operand>` | Calculates the hyperbolic arctangent of the value in register `operand` in radians and stores the result in `dest`. |
| `57` | `fcbrt` | `<u8:dest, u8:operand>` | Calculates the cube root of the value in register `operand` and stores the result in `dest`. |
| `58` | `fround` | `<u8:dest, u8:operand>` | Rounds the value in register `operand` and stores the result in `dest`. |
| `59` | `fceil` | `<u8:dest, u8:operand>` | Calculates the ceiling of the value in register `operand` and stores the result in `dest`. |
| `59` | `ffloor` | `<u8:dest, u8:operand>` | Calculates the floor of the value in register `operand` and stores the result in `dest`. |
| `5A` | `fexp` | `<u8:dest, u8:operand>` | Calculates E to the power of the value in register `operand` and stores the result in `dest`. |
| `5B` | `fln` | `<u8:dest, u8:operand>` | Calculates the natural logarithm of the value in register `operand` and stores the result in `dest`. |
| `5C` | `fpow` | `<u8:dest, u8:op1, u8:op2>` | Calculates the power of `op1` to the power of `op2` and stores the result in `dest`. |
| `70` | `call` | `<u8:func>` | Jumps to the specified function. |
| `71` | `ret` | `<>` | Returns from the current function. |
| `72` | `stret` | `<u8:src>` | Stores the value from `src` into the return register. |
| `73` | `ldret` | `<u8:dest>` | Loads the value from the return register into `dest`. |
| `74` | `starg` | `<u8:arg, u8:src>` | Stores the value from `src` into the argument register `arg`. |
| `75` | `ldarg` | `<u8:arg, u8:dest>` | Loads the value from the argument register `arg` into `src`. |
| `7F` | N/A | N/A | Extends the opcode to the next byte (e.g. 1-byte -> 2-byte, 2-byte -> 3-byte, etc.) |

> Note: It's important to notice that **not all available opcodes values are used**. These values are reserved for future use.
> Backend implementations must fail whenever unsupported opcodes are used in order to ensure a valid state.
> Extension opcodes are not supported. To request new opcodes, create an issue.
