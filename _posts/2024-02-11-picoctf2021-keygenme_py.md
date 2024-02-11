---
title: PicoCTF 2021 - KeyGenMe-py
author: KaizoSauceA
date: 2024-02-11 09:10:00 -0
categories: [CTF,PicoCTF 2021]
tags: [picoctf_reverse-engineering,python]
---

Refer to the [first entry](../picoctf2021-obedient_cat) in this series for folder structure and tips.

## Challenge Details

> **Site:** [PicoCTF](https://play.picoctf.org/)  
> **Author:** syreal  
> **Description:** keygenme-trial.py  
> **Files:** [keygenme-trial.py](https://mercury.picoctf.net/static/a6d9cac3bfa4935ceb50c145d3ff5586/keygenme-trial.py)  
> **Points:** 30  
> * *No Hints*

## Investigation

Create a folder in the relevant directory and download the py file.

```bash
cd reverse_engineering
mkdir keygenme_py && cd $_
wget https://mercury.picoctf.net/static/a6d9cac3bfa4935ceb50c145d3ff5586/keygenme-trial.py
```

Opening the py file in our code editor reveals some sort of calculator. Of course we are not very interested in the functionality, so let's look at the license and key functionality.

One of the options in the user interface is to enter a license. If we inspect that code we can see what it is doing. The license key that a user enters is passed to a **check_key** function along with the byte string **bUsername_trial** and if that check is passed, the decryption happens. Some important snippets from the code.

```python
# earlier in the code
bUsername_trial = b"PRITCHARD"

key_part_static1_trial = "picoCTF{1n_7h3_|<3y_of_"
key_part_dynamic1_trial = "xxxxxxxx"
key_part_static2_trial = "}"
key_full_template_trial = key_part_static1_trial + key_part_dynamic1_trial + key_part_static2_trial

def enter_license():
    user_key = input("\nEnter your license key: ")
    user_key = user_key.strip()

    global bUsername_trial
    
    if check_key(user_key, bUsername_trial):
        decrypt_full_version(user_key)
    else:
        print("\nKey is NOT VALID. Check your data entry.\n\n")
```

Let's investigate the check_key function.

```python
def check_key(key, username_trial):

    global key_full_template_trial

    if len(key) != len(key_full_template_trial):
        return False
    else:
        # Check static base key part --v
        i = 0
        for c in key_part_static1_trial:
            if key[i] != c:
                return False

            i += 1

        # TODO : test performance on toolbox container
        # Check dynamic part --v
        if key[i] != hashlib.sha256(username_trial).hexdigest()[4]:
            return False
        else:
            i += 1

        if key[i] != hashlib.sha256(username_trial).hexdigest()[5]:
            return False
        else:
            i += 1

        if key[i] != hashlib.sha256(username_trial).hexdigest()[3]:
            return False
        else:
            i += 1

        if key[i] != hashlib.sha256(username_trial).hexdigest()[6]:
            return False
        else:
            i += 1

        if key[i] != hashlib.sha256(username_trial).hexdigest()[2]:
            return False
        else:
            i += 1

        if key[i] != hashlib.sha256(username_trial).hexdigest()[7]:
            return False
        else:
            i += 1

        if key[i] != hashlib.sha256(username_trial).hexdigest()[1]:
            return False
        else:
            i += 1

        if key[i] != hashlib.sha256(username_trial).hexdigest()[8]:
            return False

        return True
```

### Reversing the Hash Checks

The logic check for the key length means the key is 8 characters long (the **key_part_dynamic1_trial = "xxxxxxxx"** variable). Once that check is passed there is 8 if statements checking each character in the key against a an encoded character based on the bUsername_trial argument. As we know that raw variable, this is how we can reverse engineer the string. Let's create a new file **tinker.py** and write some code to create the flag.

```python
import hashlib

# byte string username used for checks
bUsername_trial = b"PRITCHARD"

# table of all of the calculations used in the check_key() function
decrypt = [
    hashlib.sha256(bUsername_trial).hexdigest()[4],
    hashlib.sha256(bUsername_trial).hexdigest()[5],
    hashlib.sha256(bUsername_trial).hexdigest()[3],
    hashlib.sha256(bUsername_trial).hexdigest()[6],
    hashlib.sha256(bUsername_trial).hexdigest()[2],
    hashlib.sha256(bUsername_trial).hexdigest()[7],
    hashlib.sha256(bUsername_trial).hexdigest()[1],
    hashlib.sha256(bUsername_trial).hexdigest()[8]
]

# join all the decoded characters into a string, insert into the known flag and print
decoded = "".join(decrypt)
flag = "picoCTF{1n_7h3_|<3y_of_" + decoded + "}"
print(flag)

# tighter version, use the known order and list comprehension for the same result
order = [4, 5, 3, 6, 2, 7, 1, 8]

decoded = "".join(hashlib.sha256(bUsername_trial).hexdigest()[i] for i in order)
flag = "picoCTF{1n_7h3_|<3y_of_" + decoded + "}"
print(flag)
```

### Flag

> FLAG = picoCTF{1n_7h3_\|<3y_of_54ef6292}
{: .prompt-tip }

Mark our folder as solved.

```bash
cd ..
mv keygenme_py{,_SOLVED}
```