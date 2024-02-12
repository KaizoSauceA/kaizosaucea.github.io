---
title: PicoGym - PatchMe.py
author: KaizoSauceA
date: 2024-02-12 09:15:00 -0
categories: [CTF,PicoCTF 2022]
tags: [picoctf_general-skills,linux,python]
---

Refer to the [first entry](../picoctf2021-obedient_cat) in this series for folder structure and tips.

## Challenge Details

> **Site:** [PicoCTF](https://play.picoctf.org/)  
> **Author:** LT 'syreal' Jones  
> **Description:** Can you get the flag? Run this Python program in the same directory as this encrypted flag.   
> **Files:** [patchme.flag.py](https://artifacts.picoctf.net/c/202/patchme.flag.py) [flag.txt.enc](https://artifacts.picoctf.net/c/202/flag.txt.enc)  
> **Points:** 100  
> * *No Hints*  

## Investigation

Create a folder in the relevant directory, download the py file and the flag file.

```bash
cd general_skills
mkdir patchme && cd $_
wget https://artifacts.picoctf.net/c/202/patchme.flag.py
wget https://artifacts.picoctf.net/c/202/flag.txt.enc
```

Let's inspect the python file to try and see what is going on. The file executes the **level_1_pw_check()** function. All of the logic we need is in this function to work out the password. Upon running the file the user will be prompted for a password. The entry is then checked against a concatenation of four strings in the code and if it matches we will get the flag.

```python
def level_1_pw_check():

    user_pw = input("Please enter correct password for flag: ")

    if( user_pw == "ak98" + \
                   "-=90" + \
                   "adfjhgj321" + \
                   "sleuth9000"):
        print("Welcome back... your flag, user:")
        decryption = str_xor(flag_enc.decode(), "utilitarian")
        print(decryption)
        return
    print("That password is incorrect")
```

Lets run python from the terminal and use it to concatenate the four strings. We can then copy it, run the file and paste in the password to get the flag.

```bash
python
>>> "ak98" + \
...                    "-=90" + \
...                    "adfjhgj321" + \
...                    "sleuth9000"
'ak98-=90adfjhgj321sleuth9000'
exit()
python patchme.flag.py 
Please enter correct password for flag: ak98-=90adfjhgj321sleuth9000
Welcome back... your flag, user:
picoCTF{p47ch1ng_l1f3_h4ck_21d62e33}
```

### Flag

> FLAG = picoCTF{p47ch1ng_l1f3_h4ck_21d62e33}
{: .prompt-tip }

Mark our folder as solved.

```bash
cd ..
mv patchme{,_SOLVED}
```