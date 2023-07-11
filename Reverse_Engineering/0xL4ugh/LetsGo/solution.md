![2023-02-17_18-07](https://github.com/Harasisco/CTF-Competitions/assets/87074807/767720e9-86df-4ccc-af24-36125492284b)

# Solution
#### We are given a Linux binary which asks for Flag to check if it’s right:

> $ file ./chall <br> 
> ./chall: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), statically linked, Go BuildID=y6A4a-NwXzL94Cj_qOhp/msYAya9vrNn-YlLgUKWi/X7mAsTHwUnrAjfJDnZuR/DMu-wfjhxHr3lMGbL1wq, not stripped

![image](https://github.com/Harasisco/CTF-Competitions/assets/87074807/7aaef636-0476-4e47-9c8a-8f19120879dd)

#### we can see that the binary is not stripped which makes it easier to reverse.

#### In the main function we can see that there is two pathes for the program

![image](https://github.com/Harasisco/CTF-Competitions/assets/87074807/074d5c86-5ce7-4666-96c0-ce8571afa2b9)

#### In the right side we find it is comparing the result to a constant value a special string :
> u507rv78qr5t6q99941422uursv94464

#### In the left side we find it is doing somthing to our input

![image](https://github.com/Harasisco/CTF-Competitions/assets/87074807/76e55bb7-c96e-467b-992e-3d7dd33a49eb)
#### to make it simple, this program here checks where is the region of the character in the ASCII table.

![image](https://github.com/Harasisco/CTF-Competitions/assets/87074807/5d16705b-bb3e-4e21-96da-4982aec3016f)

#### and take an action based on if it’s:
+ Small Letter
+ Capital Letter
+ Number
+ Symbol

#### and the function making a decision based in the upper picture to add 10H (16 decimal) to our input if its right

![image](https://github.com/Harasisco/CTF-Competitions/assets/87074807/26773bee-c28f-49b5-8ab6-cf81f68ae05f)

#### to find our input we need to subtract the 10H from the special string we found:
```python
enc_string = "u507rv78qr5t6q99941422uursv94464"
flag = ""

for char in enc_string:
    if char.isalpha():
        flag += chr(ord(char) - 16)
    else:
        flag += char

print(f"0xL4UGH{{{flag}}}")
```
#### so the flag will be:
> 0xL4UGH{e507bf78ab5d6a99941222eebcf94464}
