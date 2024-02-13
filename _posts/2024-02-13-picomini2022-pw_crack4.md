---
title: PicoGym - PW Crack 4
author: KaizoSauceA
date: 2024-02-13 14:20:00 -0
categories: [CTF,PicoMini 2022]
tags: [picoctf_general-skills,linux,python]
---

Refer to the [first entry](../picoctf2021-obedient_cat) in this series for folder structure and tips.

## Challenge Details

> **Site:** [PicoCTF](https://play.picoctf.org/)  
> **Author:** LT 'syreal' Jones  
> **Description:** Can you crack the password to get the flag? Download the password checker here and you'll need the encrypted flag and the hash in the same directory too. There are 100 potential passwords with only 1 being correct. You can find these by examining the password checker script.   
> **Files:** [level4.py](https://artifacts.picoctf.net/c/19/level4.py) [level4.flag.txt.enc](https://artifacts.picoctf.net/c/19/level4.flag.txt.enc) [level4.hash.bin](https://artifacts.picoctf.net/c/19/level4.hash.bin)  
> **Points:** 100  
> * *Hint 1: A for loop can help you do many things very quickly.*  
> * *Hint 2: The str_xor function does not need to be reverse engineered for this challenge.*

## Investigation

Create a folder in the relevant directory, download all the files.

```bash
cd general_skills
mkdir pw_crack_4 && cd $_
wget https://artifacts.picoctf.net/c/19/level4.py
wget https://artifacts.picoctf.net/c/19/level4.flag.txt.enc
wget https://artifacts.picoctf.net/c/19/level4.hash.bin
```

If we open the level3.py file it looks very similar to the code from [PW Crack 3](../picomini2022-pw_crack3). In fact the internal operations of the py file look to be identical, except this time we have 100 possible passwords in the list. We can reuse the code we wrote previously. 

Copy the file to tinker.py. Let's remove the printing of the hashes this time unless we want to flood the terminal. We are only interested in the correct password after all.

```python
# pos_pw_list omitted due to size
print(f"Correct pw hash is: {correct_pw_hash}")

for x in pos_pw_list:
    pw = hash_pw(x)
    if pw == correct_pw_hash:
        print(f"{x} is the password")

level_4_pw_check()
```

Executing the file gives us the password, which we then enter at the prompt for the flag.

```bash
python tinker.py 
Correct pw hash is: b'\x1c\x92A_/\xc0\x8b\x0e\x8a\x0e\xbbo?!\xcd\xcc'
973a is the password
Please enter correct password for flag: 973a     
Welcome back... your flag, user:
picoCTF{fl45h_5pr1ng1ng_ae0fb77c}
```

### Flag

> FLAG = picoCTF{fl45h_5pr1ng1ng_ae0fb77c}
{: .prompt-tip }

Mark our folder as solved.

```bash
cd ..
mv pw_crack_4{,_SOLVED}
```