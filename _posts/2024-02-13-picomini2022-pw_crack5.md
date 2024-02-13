---
title: PicoMini 2022 - PW Crack 5
author: KaizoSauceA
date: 2024-02-13 15:00:00 -0
categories: [CTF,PicoMini 2022]
tags: [picoctf_general-skills,linux,python]
---

Refer to the [first entry](../picoctf2021-obedient_cat) in this series for folder structure and tips.

## Challenge Details

> **Site:** [PicoCTF](https://play.picoctf.org/)  
> **Author:** LT 'syreal' Jones  
> **Description:** Can you crack the password to get the flag? Download the password checker here and you'll need the encrypted flag and the hash in the same directory too. Here's a dictionary with all possible passwords based on the password conventions we've seen so far.   
> **Files:** [level5.py](https://artifacts.picoctf.net/c/31/level5.py) [level5.flag.txt.enc](https://artifacts.picoctf.net/c/31/level5.flag.txt.enc) [level5.hash.bin](https://artifacts.picoctf.net/c/31/level5.hash.bin) [dictionary.txt](https://artifacts.picoctf.net/c/31/dictionary.txt)  
> **Points:** 100  
> * *Hint 1: Opening a file in Python is crucial to using the provided dictionary.*  
> * *Hint 2: You may need to trim the whitespace from the dictionary word before hashing. Look up the Python string function, strip.*  
> * *Hint 3: The str_xor function does not need to be reverse engineered for this challenge.*

## Investigation

Create a folder in the relevant directory, download all the files.

```bash
cd general_skills
mkdir pw_crack_5 && cd $_
wget https://artifacts.picoctf.net/c/31/level5.py
wget https://artifacts.picoctf.net/c/31/level5.flag.txt.enc
wget https://artifacts.picoctf.net/c/31/level5.hash.bin
wget https://artifacts.picoctf.net/c/31/dictionary.txt
```

If we open the level5.py file it looks very similar to the code from [PW Crack 4](../picomini2022-pw_crack4). In fact the internal operations of the py file look to be identical, except this time we have an external dictionary file with all the possible passwords. We can reuse our previous code.  

Copy the file to tinker.py. We only need to change the way the pos_pw_list variable is created. In previous challenges it was a list. So if we open the dictionary.txt file, read the entire thing and split the lines out we will get an iterable list that can be used in the same way as previous challenges.

```python
print(f"Correct pw hash is: {correct_pw_hash}")

pos_pw_list = open("dictionary.txt", "r").read().splitlines()

for x in pos_pw_list:
    pw = hash_pw(x)
    if pw == correct_pw_hash:
        print(f"{x} is the password")

level_5_pw_check()
```

Run our file. We get the flag the same way.

```bash
python tinker.py 
Correct pw hash is: b'\x126P\xdd\x05`Xy\x18\xb3\xd7q\xcf\x0c\x01q'
9581 is the password
Please enter correct password for flag: 9581
Welcome back... your flag, user:
picoCTF{h45h_sl1ng1ng_36e992a6}
```

### Flag

> FLAG = picoCTF{h45h_sl1ng1ng_36e992a6}
{: .prompt-tip }

Mark our folder as solved.

```bash
cd ..
mv pw_crack_5{,_SOLVED}
```