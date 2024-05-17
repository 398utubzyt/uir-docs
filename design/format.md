# .uir File Format

This document defines the format for a UIR file.

- [Special Constants](#special-constants)
- [File Layout](#file-layout)
- [Type Reference Layout](#type-reference-layout)

## Special Constants

### Special Type IDs

| Hex Value | Name | Description |
|----|----|----|
| `00` | User Struct | The type is a user-defined struct. |
| `01` | `i8`  | The type is a signed 8-bit integer.  |
| `02` | `i16` | The type is a signed 16-bit integer. |
| `03` | `i32` | The type is a signed 32-bit integer. |
| `04` | `i64` | The type is a signed 64-bit integer. |
| `05` | `i128` | The type is a signed 128-bit integer. |
| `11` | `u8`  | The type is an unsigned 8-bit integer.  |
| `12` | `u16` | The type is an unsigned 16-bit integer. |
| `13` | `u32` | The type is an unsigned 32-bit integer. |
| `14` | `u64` | The type is an unsigned 64-bit integer. |
| `15` | `u128` | The type is an unsigned 128-bit integer. |
| `22` | `f16` | The type is a 16-bit floating point number. |
| `23` | `f32` | The type is a 32-bit floating point number. |
| `24` | `f64` | The type is a 64-bit floating point number. |
| `25` | `f128` | The type is a 128-bit floating point number. |
| `41` | `b8` | The type is an 8-bit boolean. |
| `42` | `b16` | The type is an 16-bit boolean. |
| `43` | `b32` | The type is an 32-bit boolean. |
| `44` | `b64` | The type is an 64-bit boolean. |
| `51` | `c8` | The type is an UTF-8 character. |
| `52` | `c16` | The type is a UTF-16 character. |
| `53` | `c32` | The type is a UTF-32 character. |
| `70` | `isize` | The type is a signed pointer-sized integer. |
| `71` | `usize` | The type is an unsigned pointer-sized integer. |
| `80` | `void`[^1] | The function with this return type does not return a value. |
| `81` | `noreturn`[^1] | The function with this return type will never return. |

[^1]: Not a valid variable type. This is a special code for function return types.

### Function Modifier Flags

| Bit Index | Name | Description |
|----|----|----|
| `0` | Hidden | This function should not be accessible outside of this code. |
| `1` | Extern | This function is implemented elsewhere. |
| `2` | Variadic | This function contains variadic parameters. |
| `3` | Instance | This function has an implicit self parameter. |

## File Layout

| Byte Size | Name | Description |
|----|----|----|
| `4` | Header | The sequence must be equal to the following in hex values: `55 49 52 21` |
| `2` | Struct Count | The total number of structs defined in this file. |
| [Struct Size](#struct-layout) x Count | Struct Layouts | Defines the used structs. |
| `2` | Union Count | The total number of structs defined in this file. |
| [Union Size](#union-layout) x Count | Union Layouts | Defines the used unions. |
| `2` | Global Field Count | The total number of structs defined in this file. |
| [Field Size](#field-layout) x Count | Global Field Layouts | Defines the used structs. |
| `2` | Function Count | The total number of functions defined in this file. |
| [Function Size](#function-layout) x Count | Function Layouts | Defines the used functions. |
| `2` | Function Body Count | The total number of function bodies defined in this file. |
| [Function Body Size](#function-body-layout) x Count | Function Body Layouts | Implements the body of every single non-extern function. |

### Struct Layout

| Byte Size | Name | Description |
|----|----|----|
| `2` | Field Count | The number of fields contained within the struct. |
| `2` | Name Length | The total number of characters in the struct name. Does not include a null character. |
| Name Length | Struct Name | The name of the struct in UTF-8 encoding. |
| [Field Size](#field-layout) x Count | Field Members | The number of fields contained within the struct. |

### Union Layout

| Byte Size | Name | Description |
|----|----|----|
| `2` | Field Count | The number of fields contained within the union. |
| `2` | Name Length | The total number of characters in the union name. Does not include a null character. |
| Name Length | Struct Name | The name of the union in UTF-8 encoding. |
| [Field Size](#field-layout) x Count | Field Members | The number of fields contained within the union. |

### Field Layout

| Byte Size | Name | Description |
|----|----|----|
| `8` | Type | The type of the field. See [Type Reference Layout](#type-reference-layout) for more info. |
| `2` | Name Length | The total number of characters in the field name. Does not include a null character. |
| Name Length | Field Name | The name of the field in UTF-8 encoding. |

### Function Layout

| Byte Size | Name | Description |
|----|----|----|
| `1` | Flags | The modifiers. See [Function Modifier Flags](#function-modifier-flags). |
| `8` | Return Type | The type of the function. See [Type Reference Layout](#type-reference-layout) for more info. |
| `1` | Parameter Count | The amount of parameters that the function requires. |
| `2` | Body ID | The ID of the function body. Ignore if this function is external. |
| `2` | Name Length | The total number of characters in the function name. Does not include a null character. |
| Name Length | Field Name | The name of the function in UTF-8 encoding. |
| [Parameter Size](#parameter-layout) x Count | Parameters | The parameters of the function. |

### Parameter Layout

| Byte Size | Name | Description |
|----|----|----|
| `8` | Type | The type of the parameter. See [Type Reference Layout](#type-reference-layout) for more info. |
| `2` | Name Length | The total number of characters in the parameter name. Does not include a null character. |
| Name Length | Field Name | The name of the parameter in UTF-8 encoding. |

### Function Body Layout

| Byte Size | Name | Description |
|----|----|----|
| `2` | Instruction Count | The amount of instructions contained within the function body. |
| `>= 2` x Count | Instructions | A block of UIR instructions. See [Instruction Opcodes](./opcodes.md) for more info. |

## Type Reference Layout

| Byte Size | Name | Description |
|----|----|----|
| `1` | Type ID | The type ID. See [Special Type IDs](#special-type-ids). |
| `1` | Indirection Level | The level of pointer indirection. |
| `2` | Struct ID | The ID of the struct. Ignore if the type is not a user struct. |
| `3` | Element Count | The amount of elements within the vector type. Set to `0` if this type is not a vector. |
| `1` | Vector Indirection | The level of pointer indirection to the vector. Ignore if this type is not a vector. |
