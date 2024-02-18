---
title: PicoCTF 2022 - UnpackMe.py
author: KaizoSauceA
date: 2024-02-13 08:00:00 -0
categories: [CTF,PicoCTF 2022]
tags: [picoctf_reverse-engineering,linux,python]
---

Refer to the [first entry](../picoctf2021-obedient_cat) in this series for folder structure and tips.

## Challenge Details

> **Site:** [PicoCTF](https://play.picoctf.org/)  
> **Author:** LT 'syreal' Jones  
> **Description:** Can you get the flag? Reverse engineer this Python program.   
> **Files:** [unpackme.flag.py](https://artifacts.picoctf.net/c/49/unpackme.flag.py)  
> **Points:** 100  
> * *No Hints*  

## Investigation

Create a folder in the relevant directory, download the py file.

```bash
cd reverse_engineering
mkdir unpackme_py && cd $_
wget https://artifacts.picoctf.net/c/49/unpackme.flag.py
```

Let's run the file and see what happens. We are prompted for a password, which we don't have.

```bash
python unpackme.flag.py 
What's the password? 1234
That password is incorrect.
```

Opening the code in a code editor we can look at what is going on.

```python
import base64
from cryptography.fernet import Fernet


payload = b'gAAAAABkzWGSzE6VQNTzvRXOXekQeW4CY6NiRkzeImo9LuYBHAYw_hagTJLJL0c-kmNsjY33IUbU2IWlqxA3Fpp9S7RxNkiwMDZgLmRlI9-lGAEW-_i72RSDvylNR3QkpJW2JxubjLUC5VwoVgH62wxDuYu1rRD5KadwTADdABqsx2MkY6fKNTMCYY09Se6yjtRBftfTJUL-LKz2bwgXNd6O-WpbfXEMvCv3gNQ7sW4pgUnb-gDVZvrLNrug_1YFaIe3yKr0Awo0HIN3XMdZYpSE1c9P4G0sMQ=='

key_str = 'correctstaplecorrectstaplecorrec'
key_base64 = base64.b64encode(key_str.encode())
f = Fernet(key_base64)
plain = f.decrypt(payload)
exec(plain.decode())
```

Let's make a copy of the py file and call it tinker.py. If we comment out the **exec(plain.decode)** to stop the password prompt and add a **print(plain)** we can change the output.

```python
plain = f.decrypt(payload)
#exec(plain.decode())
print(plain)
```

```bash
python tinker.py       
b"\npw = input('What\\'s the password? ')\n\nif pw == 'batteryhorse':\n  print('picoCTF{175_chr157m45_cd82f94c}')\nelse:\n  print('That password is incorrect.')\n\n"
```

The output is the decrypted payload byte string. The flag is revealed in the output as well as the password. Let's run the original file and pass in the password to see if it executes correctly also.

```bash
python unpackme.flag.py
What's the password? batteryhorse
picoCTF{175_chr157m45_cd82f94c}
```

### Flag

> FLAG = picoCTF{175_chr157m45_cd82f94c}
{: .prompt-tip }

Mark our folder as solved.

```bash
cd ..
mv unpackme_py{,_SOLVED}
```