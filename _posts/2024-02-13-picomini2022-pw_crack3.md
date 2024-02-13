---
title: PicoMini 2022 - PW Crack 3
author: KaizoSauceA
date: 2024-02-13 12:00:00 -0
categories: [CTF,PicoMini 2022]
tags: [picoctf_general-skills,linux,python]
---

Refer to the [first entry](../picoctf2021-obedient_cat) in this series for folder structure and tips.

## Challenge Details

> **Site:** [PicoCTF](https://play.picoctf.org/)  
> **Author:** LT 'syreal' Jones  
> **Description:** Can you crack the password to get the flag? Download the password checker here and you'll need the encrypted flag and the hash in the same directory too. There are 7 potential passwords with 1 being correct. You can find these by examining the password checker script.   
> **Files:** [level3.py](https://artifacts.picoctf.net/c/16/level3.py) [level3.flag.txt.enc](https://artifacts.picoctf.net/c/16/level3.flag.txt.enc) [level3.hash.bin](https://artifacts.picoctf.net/c/16/level3.hash.bin)  
> **Points:** 100  
> * *Hint 1: To view the level3.hash.bin file in the webshell, do: $ bvi level3.hash.bin*  
> * *Hint 2: To exit bvi type :q and press enter.*  
> * *Hint 3: The str_xor function does not need to be reverse engineered for this challenge.*

## Investigation

Create a folder in the relevant directory, download all the files.

```bash
cd general_skills
mkdir pw_crack_3 && cd $_
wget https://artifacts.picoctf.net/c/16/level3.py
wget https://artifacts.picoctf.net/c/16/level3.flag.txt.enc
wget https://artifacts.picoctf.net/c/16/level3.hash.bin
```

If we open the level3.py file it looks very similar to the code from [PW Crack 2](../picomini2022-pw_crack2). This time we also have hashing in play. Some important sections of the code are below. Important to note the **correct_pw_hash** variable that opens the hash file and is used to compare user inputs against. The **hash_pw(pw_str)** function takes the user input and converts it into a hash to compare against the file before returning the flag. There is also a list of possible passwords at the end of the file.

```python
flag_enc = open('level3.flag.txt.enc', 'rb').read()
correct_pw_hash = open('level3.hash.bin', 'rb').read()

def hash_pw(pw_str):
    pw_bytes = bytearray()
    pw_bytes.extend(pw_str.encode())
    m = hashlib.md5()
    m.update(pw_bytes)
    return m.digest()

def level_3_pw_check():
    user_pw = input("Please enter correct password for flag: ")
    user_pw_hash = hash_pw(user_pw)

    if( user_pw_hash == correct_pw_hash ):
        print("Welcome back... your flag, user:")
        decryption = str_xor(flag_enc.decode(), user_pw)
        print(decryption)
        return
    print("That password is incorrect")

pos_pw_list = ["6997", "3ac8", "f0ac", "4b17", "ec27", "4e66", "865e"]
```

### Coding Up a Solution

We could brute force this by running the program and entering the list of passwords until we find the right one. However, let's create a copy of the py file as tinker.py and write some code to iterate through all of the possible passwords, sending them to the hash_pw function and comparing so we can work out the correct password.

```python
print(f"Correct pw hash is: {correct_pw_hash}")

pos_pw_list = ["6997", "3ac8", "f0ac", "4b17", "ec27", "4e66", "865e"]

for x in pos_pw_list:
    pw = hash_pw(x)
    if pw == correct_pw_hash:
        print(f"{x} hash is: {pw} (CORRECT)")
    else:
        print(f"{x} hash is: {pw}")

level_3_pw_check()
```

Adding the above code at the end of the tinker.py file will print out the correct has, then check all the possibles in the list (and tell us which matches) before finally resuming normal functionality, at which point we type the correct string and get the flag.

```bash
python tinker.py
Correct pw hash is: b'\x1b\x18\xe11o\x92\x18\xcc[\x05>\x1c\xea(\xe0.'
6997 hash is: b'\xc9\x04\x9d*F\xfe\xb0\xae-\xe6\xb0co2\xea\r'
3ac8 hash is: b"\x96H\xae#K\xd3'\xd6\x86\xf9\xd6\x844 p\xcd"
f0ac hash is: b'\xeewQ\xa0]\xea\xd0\x1a67\x87)\xfd\x0c N'
4b17 hash is: b'\x03\x07\x81y\xea>\x83\x07V \xb5\xed\x18\xf9X\x94'
ec27 hash is: b'\xf4\x17\x198\x89\x02\xe5\x1c\x11\x1aWx\xb4f\x9f\xcb'
4e66 hash is: b'\xcdi%\xa9\xd52E\xb1\x06\xa9~0?^.\xf7'
865e hash is: b'\x1b\x18\xe11o\x92\x18\xcc[\x05>\x1c\xea(\xe0.' (CORRECT)
Please enter correct password for flag: 865e 
Welcome back... your flag, user:
picoCTF{m45h_fl1ng1ng_2b072a90}
```

### Flag

> FLAG = picoCTF{m45h_fl1ng1ng_2b072a90}
{: .prompt-tip }

Mark our folder as solved.

```bash
cd ..
mv pw_crack_3{,_SOLVED}
```