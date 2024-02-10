---
title: PicoGym - ASCII Numbers
author: KaizoSauceA
date: 2024-02-10 11:30:00 -0
categories: [CTF,PicoGym]
tags: [picoctf_general-skills,linux,python]
---

Refer to the [first entry](../picoctf2021-obedient_cat) in this series for folder structure and tips.

## Challenge Details

> **Site:** [PicoCTF](https://play.picoctf.org/)  
> **Author:** LT 'syreal' Jones  
> **Description:** Convert the following string of ASCII numbers into a readable string: 0x70 0x69 0x63 0x6f 0x43 0x54 0x46 0x7b 0x34 0x35 0x63 0x31 0x31 0x5f 0x6e 0x30 0x5f 0x71 0x75 0x33 0x35 0x37 0x31 0x30 0x6e 0x35 0x5f 0x31 0x6c 0x6c 0x5f 0x74 0x33 0x31 0x31 0x5f 0x79 0x33 0x5f 0x6e 0x30 0x5f 0x6c 0x31 0x33 0x35 0x5f 0x34 0x34 0x35 0x64 0x34 0x31 0x38 0x30 0x7d   
> **Files:** N/A 
> **Points:** 100  
> * *Hint 1: CyberChef is a great tool for any encoding but especially ASCII.*
> * *Hint 2: Try CyberChef's 'From Hex' function*

## Investigation

Create a folder in the relevant directory and create a text file we can paste the string into. Open the text file and paste in the data.

```bash
cd general_skills
mkdir ascii_numbers && cd $_
mousepad data.txt
```

### CyberChef

If we go to [CyberChef](https://cyberchef.org/), drag in the **From Hex** operation to the recipe section and paste our data into the input, the flag is generated for us.

### Python

Let's try and accomplish the same in python. Using the [FavTutor](https://favtutor.com/blogs/hex-to-string-in-python) site as a useful resource.

```python
import codecs

# read the data file, replace the 0x characters, strip the newline and split on spaces
with open("data.txt", "r") as file:
    raw = file.read().replace("0x", "").strip().split(" ")

# iterate over the clean data, decoding each character to print the flag
for x in raw:
    print(codecs.decode(x, "hex").decode("utf-8"), end="")
```

```bash
python decode.py
picoCTF{45c11_n0_qu35710n5_1ll_t311_y3_n0_l135_445d4180}
```

### Flag

> FLAG = picoCTF{45c11_n0_qu35710n5_1ll_t311_y3_n0_l135_445d4180}
{: .prompt-tip }

Mark our folder as solved.

```bash
cd ..
mv ascii_numbers{,_SOLVED}
```