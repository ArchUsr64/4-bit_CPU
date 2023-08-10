# 4-bit CPU
A 4-bit CPU using the harvard architecture built on [simulator.io](https://simulator.io/board/qrKHHvNRzY/3)
![image](https://github.com/ArchUsr64/4-bit_CPU/assets/83179501/737b98fe-dfac-41f9-b609-8875263a3513)

## Specification
- 4x4-bit RAM
- 16x4-bit PROM
- 4-bit Accumulator
- Hardware arithmetic support:
  - ADD
  - NAND

## Architecture Details
The system starts by initializing the registers by copying values
from the user programmable 4x4-bit ROM array in first four clock cycles
Following each group of four clock cycles executes one instruction from
the PROM, starting from top-left for the first instruction.

The two most significant bits denote the opcode for the operation and
the remaining two represent the RAM address  
`[2-bit opcode] [2-bit RAM address]`

## Supported Instructions
Instruction format: `[OpCode]XX`
| OpCode | Instruction | Expression | Description
|    -   |      -      |     -      |      -
| 00 | Add   | `ACM += Value at Register XX`        | Adds the accumulator value with the value stored at specified Register
| 01 | NAND  | `ACM = !(ACM & Value at Register XX` | NANDs the accumulator value with the value stored at specified Registers
| 10 | Load  | `ACM = Value at Register XX`         | Loads the value from the specified Register to the accumulator
| 11 | Store | `Value at Register XX = ACM`         | Stores the value from the specified Register into the accumulator

**Example:** Instruction `1011` will Load the value from Register number 3 to accumuator

## Program Example
To subtract two numbers 4-bit numbers `A` and `B` using the 2's complement method:  
### Operation: `R3 = A - B`
1. RAM initial values:
   | Register | Value |
   |    -     |   -   |
   |  R0    |  A  |
   |  R1    |  B  |
   |  R2    |  1  |
   |  R3    |  -  |
2. Instructions:
   | Instruction | Mnemonic | Description |
   |      -      |    -     |     -       |
   | 1001 | Load R1 | Load the value of `B` to the accumulator |
   | 0101 | NAND R1 | NAND the accumulator(`B`) with R1(`B`), essentially computing NOT of `B` |
   | 0010 | ADD R2 | Add `1` to the value of accumulator(NOT of `A`) which gives the 2's complement of `B` |
   | 0000 | ADD R0 | Add 'A' with value at accumulator(2's complement of `B`) |
   | 1111 | Store R3 | Stores the result of previously computed `A + 2's complement of B` to R3 |
3. After execution of 22 clock cycles, the output should be computed in `R3`
