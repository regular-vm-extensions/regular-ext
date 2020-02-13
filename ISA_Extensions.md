# Extensions to the REGULAR Instruction Set Architecture
This document describes REGULAR-EXT, a set of extensions to the [REGULAR ISA](https://github.com/regular-vm/specification) designed to expand the range of computing tasks for which REGULAR is suited and to increase the ease of developing programs intended to run on REGULAR implementations. The inclusion of a feature in REGULAR-EXT signifies the regular-vm-extensions working group's belief that the feature is sufficiently critical to the portability and interoperability of REGULAR programs to justify the use of a shared design across implementations.

## Instructions
REGULAR-EXT supplements the REGULAR specification with the following additional instructions, to be implemented at a hardware level.

| Name  | Encoding   | Description                                                    |
|-------|------------|----------------------------------------------------------------|
| `hlt` | 0xff       | Halt execution.                                                |
| `snd` | 0xfd rA rB | Send the command in rB to the device whose ID is stored in rA. | 
