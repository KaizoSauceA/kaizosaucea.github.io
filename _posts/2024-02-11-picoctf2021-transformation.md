---
title: PicoCTF 2021 - Transformation
author: KaizoSauceA
date: 2024-02-11 10:10:00 -0
categories: [CTF,PicoCTF 2021]
tags: [picoctf_reverse-engineering,linux,python]
---

Refer to the [first entry](../picoctf2021-obedient_cat) in this series for folder structure and tips.

## Challenge Details

> **Site:** [PicoCTF](https://play.picoctf.org/)  
> **Author:** madStacks  
> **Description:** I wonder what this really is... enc  
''.join([chr((ord(flag[i]) << 8) + ord(flag[i + 1])) for i in range(0, len(flag), 2)])  
> **Files:** [enc](https://mercury.picoctf.net/static/2b4cea9b07db22bf4f933fddd1b8caa9/enc)  
> **Points:** 20  
> * *Hint 1: You may find some decoders online*

## Investigation

Create a folder in the relevant directory and download the file. If we cat the enc file we get a sequence of characters.

```bash
cd reverse_engineering
mkdir transformation && cd $_
wget https://mercury.picoctf.net/static/2b4cea9b07db22bf4f933fddd1b8caa9/enc
cat enc
灩捯䍔䙻ㄶ形楴獟楮獴㌴摟潦弸弰㑣〷㘰摽
```

If we go to [CyberChef](https://cyberchef.org/) and use the **magic** setting, paste in the characters and play around we can see what happens. Once changing the input encoding to **UTF-16LE** we get something resembling a familiar flag: ipocTC{F61b_ti_snits43_dfo8_0_c47006}d. Changing encoding to **UTF-16BE** we get the flag picoCTF{16_bits_inst34d_of_8_04c0760d}.

### Python Builtin Method

Let's see if we can solve this in python to further our understanding. Create a new file decode.py

```python
# read the enc file into variable raw
with open("enc", "r") as file:
    raw = file.read()

# print using the utf-16-be encoding we found on CyberChef
print(raw.encode('utf-16-be'))
```

Running our file we get the flag

```bash
python decode.py        
b'picoCTF{16_bits_inst34d_of_8_04c0760d}'
```

### Python Manual Method

```python
# read the enc file into variable raw
with open("enc", "r") as file:
    raw = file.read()

# We know the flag is: picoCTF{16_bits_inst34d_of_8_04c0760d}
# from the challenge:
# ''.join([chr((ord(flag[i]) << 8) + ord(flag[i + 1])) for i in range(0, len(flag), 2)])
#
# the encoding is doing "chr((ord(flag[i]) << 8) + ord(flag[i + 1]" for every 2
# characters in the flag. we need to reverse this by taking single characters
# from the enc file and splitting them into 2 while doing the reverse steps

flag = ""

for i in range(len(raw)):
    
    # shift right to reverse left shift
    char1 = chr(ord(raw[i]) >> 8)
    # recover the second character
    char2 = chr(raw[i].encode('utf-16-be')[-1])

    # join the recovered characters and append to the flag string
    flag += char1 + char2

print(flag)
```

```bash
python decode.py
picoCTF{16_bits_inst34d_of_8_04c0760d}
```

### Flag

> FLAG = picoCTF{16_bits_inst34d_of_8_04c0760d}
{: .prompt-tip }

Mark our folder as solved.

```bash
cd ..
mv transformation{,_SOLVED}
```