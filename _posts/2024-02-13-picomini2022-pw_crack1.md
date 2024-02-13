---
title: PicoGym - PW Crack 1
author: KaizoSauceA
date: 2024-02-13 09:00:00 -0
categories: [CTF,PicoMini 2022]
tags: [picoctf_general-skills,linux,python]
---

Refer to the [first entry](../picoctf2021-obedient_cat) in this series for folder structure and tips.

## Challenge Details

> **Site:** [PicoCTF](https://play.picoctf.org/)  
> **Author:** LT 'syreal' Jones  
> **Description:** Can you crack the password to get the flag? Download the password checker here and you'll need the encrypted flag in the same directory too.   
> **Files:** [level1.py](https://artifacts.picoctf.net/c/10/level1.py) [level1.flag.txt.enc](https://artifacts.picoctf.net/c/10/level1.flag.txt.enc)  
> **Points:** 100  
> * *Hint 1: To view the file in the webshell, do: $ nano level1.py*  
> * *Hint 2: To exit nano, press Ctrl and x and follow the on-screen prompts.*  
> * *Hint 3: The str_xor function does not need to be reverse engineered for this challenge.*  

## Investigation

Create a folder in the relevant directory, download the py file and flag file.

```bash
cd general_skills
mkdir pw_crack_1 && cd $_
wget https://artifacts.picoctf.net/c/10/level1.py
wget https://artifacts.picoctf.net/c/10/level1.flag.txt.enc
```

If we open the level1.py file the password is in plain text as part of the level_1_pw_check() function.

```python
def level_1_pw_check():

    user_pw = input("Please enter correct password for flag: ")
    if( user_pw == "691d"):
        print("Welcome back... your flag, user:")
        decryption = str_xor(flag_enc.decode(), user_pw)
        print(decryption)
        return
    print("That password is incorrect")
```

Let's run the file and pass in the password. We have the flag.

```bash
python level1.py    
Please enter correct password for flag: 691d
Welcome back... your flag, user:
picoCTF{545h_r1ng1ng_56891419}
```

### Flag

> FLAG = picoCTF{545h_r1ng1ng_56891419}
{: .prompt-tip }

Mark our folder as solved.

```bash
cd ..
mv pw_crack_1{,_SOLVED}
```