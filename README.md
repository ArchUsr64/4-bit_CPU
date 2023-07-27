# 4-bit CPU
A 4-bit CPU using the harvard architecture built on simulator.io  
Checkout the live demo [here](https://simulator.io/board/qrKHHvNRzY/3)

### Specification:
- 4x4-bit RAM
- 16x4-bit PROM
- 4-bit Accumulator
- Hardware arithmetic support:
  - ADD
  - NAND

### Architecture Details:
The system starts by initializing the registers by copying values
from the user programmable 4x4-bit ROM array in first four clock cycles
Following each group of four clock cycles executes one instruction from
the PROM, starting from top-left for the first instruction.

The two most significant bits denote the opcode for the operation and
the remaining two represent the RAM address  
`[2-bit opcode] [2-bit RAM address]`

### Supported Instructions:
Instruction format: `[OpCode]XX`
| OpCode | Instruction | Expression | Description
|    -   |      -      |     -      |      -
| 00 | Add   | `ACM += Value at Register XX`        | Adds the accumulator value with the value stored at specified Register
| 01 | NAND  | `ACM = !(ACM & Value at Register XX` | NANDs the accumulator value with the value stored at specified Registers
| 10 | Load  | `ACM = Value at Register XX`         | Loads the value from the specified Register to the accumulator
| 11 | Store | `Value at Register XX = ACM`         | Stores the value from the specified Register into the accumulator

**Example:** Instruction `1011` will Store the value from Register number 3 to accumuator
