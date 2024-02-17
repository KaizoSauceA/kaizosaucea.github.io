---
title: PicoCTF 2022 - Basic-Mod 1
author: KaizoSauceA
date: 2024-02-17 10:00:00 -0
categories: [CTF,PicoCTF 2022]
tags: [picoctf_cryptography,linux,python]
---

Refer to the [first entry](../picoctf2021-obedient_cat) in this series for folder structure and tips.

## Challenge Details

> **Site:** [PicoCTF](https://play.picoctf.org/)  
> **Author:** Will Hong  
> **Description:** We found this weird message being passed around on the servers, we think we have a working decryption scheme. Download the message here. Take each number mod 37 and map it to the following character set: 0-25 is the alphabet (uppercase), 26-35 are the decimal digits, and 36 is an underscore. Wrap your decrypted message in the picoCTF flag format (i.e. picoCTF{decrypted_message}).   
> **Files:** [message.txt](https://artifacts.picoctf.net/c/129/message.txt)  
> **Points:** 100  
> * *Hint 1: Do you know what mod 37 means?*  
> * *Hint 2: mod 37 means modulo 37. It gives the remainder of a number after being divided by 37.*

## Investigation

Create a folder in the relevant directory, download the txt file.

```bash
cd cryptography
mkdir basic_mod1 && cd $_
wget https://artifacts.picoctf.net/c/129/message.txt
```

Looking at the txt file with cat reveals a sequence of numbers. Let's open python and see what we can do. The notes included below should explain the logic of what we are doing.

```python
# open the txt file. read contents, strip spaces and newline
# and finally split on the spaces to create a list of numbers
with open("message.txt", "r") as file:
    raw = file.read().strip().split(" ")

# create a string (array) to map against
# alphabet uppercase (0-25), numbers 0-9 (26-35) and _ (36)
mapped = "ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789_"

# placeholder string to append decoded characters to
decoded = ""

# iterate over the numbers, modulus 37 and use the resulting
# integer to index into mapped. add to decoded string
for i in raw:
    new = int(i) % 37
    decoded += mapped[new]

# print out the flag in the correct format
print("picoCTF{" + decoded + "}")
```

Running our new file gives us the flag, already formatted.

```bash
python tinker.py
picoCTF{R0UND_N_R0UND_ADD17EC2}
```

### Flag

> FLAG = picoCTF{R0UND_N_R0UND_ADD17EC2}
{: .prompt-tip }

Mark our folder as solved.

```bash
cd ..
mv basic_mod1{,_SOLVED}
```