# Extensions to the REGULAR Instruction Set Architecture
This ISA extension supplements the REGULAR specification with the following additional instructions, to be implemented at a hardware level.

| Name  | Encoding      | Description                                                                                  |
|-------|---------------|----------------------------------------------------------------------------------------------|
| `hlt` | 0xff          | Halt execution.                                                                              |
| `snd` | 0x?? rA rB rC | Send the command in rC to the device whose ID is stored in rB. The device *may* write to rA. | 

