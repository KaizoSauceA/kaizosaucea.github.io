---
title: PicoGym - PW Crack 2
author: KaizoSauceA
date: 2024-02-13 10:20:00 -0
categories: [CTF,PicoMini 2022]
tags: [picoctf_general-skills,linux,python]
---

Refer to the [first entry](../picoctf2021-obedient_cat) in this series for folder structure and tips.

## Challenge Details

> **Site:** [PicoCTF](https://play.picoctf.org/)  
> **Author:** LT 'syreal' Jones  
> **Description:** Can you crack the password to get the flag? Download the password checker here and you'll need the encrypted flag in the same directory too.   
> **Files:** [level2.py](https://artifacts.picoctf.net/c/15/level2.py) [level2.flag.txt.enc](https://artifacts.picoctf.net/c/15/level2.flag.txt.enc)  
> **Points:** 100  
> * *Hint 1: Does that encoding look familiar?*  
> * *Hint 2: The str_xor function does not need to be reverse engineered for this challenge.*  

## Investigation

Create a folder in the relevant directory, download the py file and flag file.

```bash
cd general_skills
mkdir pw_crack_2 && cd $_
wget https://artifacts.picoctf.net/c/15/level2.py
wget https://artifacts.picoctf.net/c/15/level2.flag.txt.enc
```

If we open the level2.py file it looks very similar to the code from [PW Crack 1](../picomini2022-pw_crack1) except this time not in plain text, but encoded characters. 

```python
def level_2_pw_check():
    user_pw = input("Please enter correct password for flag: ")
    if( user_pw == chr(0x33) + chr(0x39) + chr(0x63) + chr(0x65) ):
        print("Welcome back... your flag, user:")
        decryption = str_xor(flag_enc.decode(), user_pw)
        print(decryption)
        return
    print("That password is incorrect")
```

Let's run python from the command line and paste in the characters. We now have the plain text.

```bash
python
>>> chr(0x33) + chr(0x39) + chr(0x63) + chr(0x65)
'39ce'
```

If we run the python file and pass in the password we should get the flag.

```bash
python level2.py 
Please enter correct password for flag: 39ce
Welcome back... your flag, user:
picoCTF{tr45h_51ng1ng_502ec42e}
```

### Flag

> FLAG = picoCTF{tr45h_51ng1ng_502ec42e}
{: .prompt-tip }

Mark our folder as solved.

```bash
cd ..
mv pw_crack_2{,_SOLVED}
```