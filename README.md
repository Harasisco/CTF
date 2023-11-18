# CTF-Competitions Notes

- function (value1, value2, value3,...etc)
  - RDI: First arg
  - RSI: Second arg
  - RDX: Third arg
  - RCX: 4th arg
  - RAX: Return value
 
- To read a register content `i registers <register>`

- When the file is (PIE), then it will have a different address each time you run it to handle that:
  - Get the offset from static analysis then:
  - use `piebase` to see the base address of the binary
  - use `breakva 0xoffset` to set a breakpoint
 
- To see the memory map use the command `vmmap`
  - you can use the address you got to rebase the addresses in ghidra by going to `window -> memory map -> home icon -> write the address you got from gdb`


