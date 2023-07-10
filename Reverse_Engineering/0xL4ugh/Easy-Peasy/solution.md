![image](https://github.com/Harasisco/CTF/assets/87074807/2eda2bdd-2fd2-47f7-ab11-ba85ef8c4f62 "Description")

## solution
Once u run the chall.exe file the program will ask u for the flag

![image](https://github.com/Harasisco/CTF/assets/87074807/ef3d5bb8-1de1-4851-b7f9-c06f2ec327d7 "User Input")

I created an IDA project to start our reverse engineering process.

I found the main code:

```C
int __cdecl main(int argc, const char **argv, const char **envp)
{
  __int64 v3; // rbx
  void **v4; // rdx
  void **v5; // rax
  __int64 v6; // rax
  void **v7; // rdx
  __int64 v8; // rax
  void *v9; // rcx
  int v11[6]; // [rsp+20h] [rbp-50h]
  __int16 v12; // [rsp+38h] [rbp-38h]
  char v13; // [rsp+3Ah] [rbp-36h]
  __int64 v14; // [rsp+40h] [rbp-30h]
  void *Block[2]; // [rsp+48h] [rbp-28h] BYREF
  __int64 v16; // [rsp+58h] [rbp-18h]
  unsigned __int64 v17; // [rsp+60h] [rbp-10h]

  v14 = -2i64;
  v11[0] = 1947518052;
  v11[1] = 84227255;
  v11[2] = -181070859;
  v11[3] = -972881100;
  v11[4] = 1396909045;
  v11[5] = 1396929315;
  v12 = -10397;
  v13 = 0;
  v3 = 0i64;
  v16 = 0i64;
  v17 = 15i64;
  LOBYTE(Block[0]) = 0;
  sub_140001350(Block);
  sub_1400015D0(std::cout, "Enter The Flag: ");
  sub_140001A50(std::cin, Block);
  if ( v16 == 26 )
  {
    while ( 1 )
    {
      v4 = Block;
      if ( v17 >= 0x10 )
        v4 = (void **)Block[0];
      v5 = Block;
      if ( v17 >= 0x10 )
        v5 = (void **)Block[0];
      if ( *((unsigned __int8 *)v11 + v3) != ((*((char *)v4 + v3) >> 4) | (16 * (*((_BYTE *)v5 + v3) & 0xF))) )
        break;
      if ( ++v3 >= 26 )
      {
        v6 = sub_1400015D0(std::cout, "The Flag is: ");
        v7 = Block;
        if ( v17 >= 0x10 )
          v7 = (void **)Block[0];
        sub_140001C50(v6, v7, v16);
        goto LABEL_12;
      }
    }
  }
  v8 = sub_1400015D0(std::cout, "This will not work");
  std::ostream::operator<<(v8, sub_1400017A0);
LABEL_12:
  if ( v17 >= 0x10 )
  {
    v9 = Block[0];
    if ( v17 + 1 >= 0x1000 )
    {
      v9 = (void *)*((_QWORD *)Block[0] - 1);
      if ( (unsigned __int64)(Block[0] - v9 - 8) > 0x1F )
        invalid_parameter_noinfo_noreturn();
    }
    j_j_free(v9);
  }
  return 0;
}
```

 Our input length is compared at the start with “26” So we now know the length of the flag.

>   if ( v16 == 26 )

In disassembly section for the line:

>  if ( *((unsigned __int8 *)v11 + v3) != ((*((char *)v4 + v3) >> 4) | (16 * (*((_BYTE *)v5 + v3) & 0xF))) )

We find it make a **SHL** operation with 4 for our input and compared it with a specific address

![image](https://github.com/Harasisco/CTF/assets/87074807/92c095f1-ed94-4810-a797-5454ccd30583)

Jumping to x64dbg to debuge the loop:

![image](https://github.com/Harasisco/CTF/assets/87074807/8baca5ae-29bd-4b0e-8172-31472187f4a0 "Debuge the Loop")

the reference that our input will be compared with to get our flag.

![image](https://github.com/Harasisco/CTF/assets/87074807/25f1ffa2-24b6-4fd2-8d1a-62918a17eece)

> [64 C4 14 74 B7 34 05 05 F5 13 35 F5 34 03 03 C6 F5 23 43 53 23 73 43 53 63 D7]

writing a python code to reverse engineering the values:

```python
input_bytes = [0x64, 0xC4, 0x14, 0x74, 0xB7, 0x34, 0x05, 0x05, 0xF5, 0x13,
               0x35, 0xF5, 0x34, 0x03, 0x03, 0xC6, 0xF5, 0x23, 0x43, 0x53,
               0x23, 0x73, 0x43, 0x53, 0x63, 0xD7]

# Byte array to be populated with results
result = []

for byte in input_bytes:
    high = (byte & 0xF0) >> 4  # High nibble.
    low = (byte & 0x0F) << 4  # Low nibble.
    result_byte = high | low  # Swap nibbles.
    result.append(result_byte)

# Convert bytes to string
result_string = ''.join([chr(b) for b in result])

print(result_string)
```

and we got the flag.





