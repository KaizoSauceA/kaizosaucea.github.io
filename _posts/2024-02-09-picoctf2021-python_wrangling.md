---
title: PicoCTF 2021 - Python Wrangling
author: KaizoSauceA
date: 2024-02-09 13:00:00 -0
categories: [CTF,PicoCTF 2021]
tags: [picoctf_general-skills,linux,python]
---

Refer to the [first entry](../picoctf2021-obedient_cat) in this series for folder structure and tips.

## Challenge Details

> **Site:** [PicoCTF](https://play.picoctf.org/)  
> **Author:** syreal  
> **Description:** Python scripts are invoked kind of like programs in the Terminal... Can you run this Python script using this password to get the flag?  
> **Files:** [ende.py](https://mercury.picoctf.net/static/8e33ede04d02f3765b8c6a6e24d72733/ende.py), [pwd.txt](https://mercury.picoctf.net/static/8e33ede04d02f3765b8c6a6e24d72733/pw.txt), [flag.txt.en](https://mercury.picoctf.net/static/8e33ede04d02f3765b8c6a6e24d72733/flag.txt.en)  
> **Points:** 10  
> * *Hint 1: Get the Python script accessible in your shell by entering the following command in the Terminal prompt: $ wget https://mercury.picoctf.net/static/8e33ede04d02f3765b8c6a6e24d72733/ende.py*
> * *Hint 2: $ man python*

## Investigation

Create a folder in the relevant directory and download all the files for the challenge.

```bash
cd general_skills
mkdir python_wrangling && cd $_
wget https://mercury.picoctf.net/static/8e33ede04d02f3765b8c6a6e24d72733/ende.py
wget https://mercury.picoctf.net/static/8e33ede04d02f3765b8c6a6e24d72733/pw.txt
wget https://mercury.picoctf.net/static/8e33ede04d02f3765b8c6a6e24d72733/flag.txt.en
```

Use **cat** to read the pwd.txt file and open the python file

```bash
cat pw.txt                   
aa821c16aa821c16aa821c16aa821c16
code ende.py
```

Reading though the python file we see the **help_msg** variable.

```python
import sys
import base64
from cryptography.fernet import Fernet


usage_msg = "Usage: "+ sys.argv[0] +" (-e/-d) [file]"
help_msg = usage_msg + "\n" +\
        "Examples:\n" +\
        "  To decrypt a file named 'pole.txt', do: " +\
        "'$ python "+ sys.argv[0] +" -d pole.txt'\n"
```

Following the instructions from the bash shell we run python, **argv[0]** is the filename **-d** is decode and **flag.txt.en** is the file we wish to decode. We are prompted for the password which we have from the previous step. Paste that in and we have the decrypted flag.

```bash
python ende.py -d flag.txt.en            
Please enter the password:aa821c16aa821c16aa821c16aa821c16
picoCTF{4p0110_1n_7h3_h0us3_aa821c16}
```

### Breakdown

The relevant section in the python file is the check for **argv[1]** being **-d**. Reading the **open** call shows decrypting. We also see that the password prompt is a result of the arguments being less than 4. The **else** statement shows we can pass the password string in as an argument also.

```python
elif sys.argv[1] == "-d":
    if len(sys.argv) < 4:
        sim_sala_bim = input("Please enter the password:")
    else:
        sim_sala_bim = sys.argv[3]

    ssb_b64 = base64.b64encode(sim_sala_bim.encode())
    c = Fernet(ssb_b64)

    with open(sys.argv[2], "r") as f:
        data = f.read()
        data_c = c.decrypt(data.encode())
        sys.stdout.buffer.write(data_c)
```

Command line including the password string for the same result without the prompt.

```bash
python ende.py -d flag.txt.en aa821c16aa821c16aa821c16aa821c16
picoCTF{4p0110_1n_7h3_h0us3_aa821c16}
```

### Flag

> FLAG = picoCTF{4p0110_1n_7h3_h0us3_aa821c16}
{: .prompt-tip }

Mark our folder as solved.

```bash
cd ..
mv python_wrangling{,_SOLVED}
```