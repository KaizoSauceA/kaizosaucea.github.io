---
title: PicoCTF 2021 - CrackMe-py
author: KaizoSauceA
date: 2024-02-11 08:00:00 -0
categories: [CTF,PicoCTF 2021]
tags: [picoctf_reverse-engineering,python]
---

Refer to the [first entry](../picoctf2021-obedient_cat) in this series for folder structure and tips.

## Challenge Details

> **Site:** [PicoCTF](https://play.picoctf.org/)  
> **Author:** syreal  
> **Description:** crackme.py  
> **Files:** [crackme.py](https://mercury.picoctf.net/static/fd0e358d4b82695c220c0d6013c11484/crackme.py)  
> **Points:** 30  
> * *No Hints*

## Investigation

Create a folder in the relevant directory and download the py file.

```bash
cd reverse_engineering
mkdir crackme_py && cd $_
wget https://mercury.picoctf.net/static/fd0e358d4b82695c220c0d6013c11484/crackme.py
```

Opening the file reveals a program set to call a function **choose_greatest()**, but there is another unused function **decode_secret(secret)**. Let's create a new file called tinker.py so we can experiment.

If we remove all the code related to the choose_greatest function, and instead call the **decode_secret** function passing in the **bezos_cc_secret** as the argument we should make progress. All the comments have been removed for simplicity below.

```python
bezos_cc_secret = "A:4@r%uL`M-^M0c0AbcM-MFE055a4ce`eN"

alphabet = "!\"#$%&'()*+,-./0123456789:;<=>?@ABCDEFGHIJKLMNOPQRSTUVWXYZ"+ \
            "[\\]^_`abcdefghijklmnopqrstuvwxyz{|}~"

def decode_secret(secret):
    rotate_const = 47

    decoded = ""

    for c in secret:
        index = alphabet.find(c)
        original_index = (index + rotate_const) % len(alphabet)
        decoded = decoded + alphabet[original_index]

    print(decoded)

decode_secret(bezos_cc_secret)
```

Let's run our new file. We have the flag.

```bash
python tinker.py 
picoCTF{1|\/|_4_p34|\|ut_dd2c4616}
```

### Flag

> FLAG = picoCTF{1\|\/\|_4_p34\|\\|ut_dd2c4616}
{: .prompt-tip }

Mark our folder as solved.

```bash
cd ..
mv crackme_py{,_SOLVED}
```