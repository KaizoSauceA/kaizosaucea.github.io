---
title: PicoCTF 2022 - Basic-Mod 2
author: KaizoSauceA
date: 2024-02-17 10:50:00 -0
categories: [CTF,PicoCTF 2022]
tags: [picoctf_cryptography,linux,python]
---

Refer to the [first entry](../picoctf2021-obedient_cat) in this series for folder structure and tips.

## Challenge Details

> **Site:** [PicoCTF](https://play.picoctf.org/)  
> **Author:** Will Hong  
> **Description:** A new modular challenge! Download the message here. Take each number mod 41 and find the modular inverse for the result. Then map to the following character set: 1-26 are the alphabet, 27-36 are the decimal digits, and 37 is an underscore. Wrap your decrypted message in the picoCTF flag format (i.e. picoCTF{decrypted_message}).   
> **Files:** [message.txt](https://artifacts.picoctf.net/c/179/message.txt)  
> **Points:** 100  
> * *Hint 1: Do you know what the modular inverse is?*  
> * *Hint 2: The inverse modulo z of x is the number, y that when multiplied by x is 1 modulo z.*
> * *Hint 3: It's recommended to use a tool to find the modular inverses.*

## Investigation

Create a folder in the relevant directory, download the txt file.

```bash
cd cryptography
mkdir basic_mod2 && cd $_
wget https://artifacts.picoctf.net/c/179/message.txt
```

The is very similar to [basic-mod1](../picoctf2022-basic_mod1) so we can repurpose a lot of the python code written for that. We just need to work out how to calculate the modular inverse and add that logic to get the flag.

If we look at the [python docs](https://docs.python.org/3/library/functions.html#pow) for the pow() function we can see how to achieve this. Let's implement it into our code.

We also need to adjust the indexing of the string as this challenge starts at 1 instead of 0.

```python
# open the txt file. read contents, strip spaces and newline
# and finally split on the spaces to create a list of numbers
with open("message.txt", "r") as file:
    raw = file.read().strip().split(" ")

# create a string (array) to map against
# alphabet uppercase (1-26), numbers 0-9 (27-36) and _ (37)
mapped = "ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789_"

# placeholder string to append decoded characters to
decoded = ""

# iterate over the numbers, calculate modular inverse and
# index into mapped (subtracting 1). add to decoded
for i in raw:
    new = pow(int(i), -1, 41)
    decoded += mapped[new -1]

# print out the flag in the correct format
print("picoCTF{" + decoded + "}")
```

Running our new file gives us the flag, already formatted.

```bash
python tinker.py
picoCTF{1NV3R53LY_H4RD_DADAACAA}
```

### Flag

> FLAG = picoCTF{1NV3R53LY_H4RD_DADAACAA}
{: .prompt-tip }

Mark our folder as solved.

```bash
cd ..
mv basic_mod2{,_SOLVED}
```