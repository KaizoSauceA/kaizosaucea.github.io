---
title: PicoGym - Picker I
author: KaizoSauceA
date: 2024-02-10 13:40:00 -0
categories: [CTF,PicoGym]
tags: [picoctf_reverse-engineering,linux,python]
---

Refer to the [first entry](../picoctf2021-obedient_cat) in this series for folder structure and tips.

## Challenge Details

> **Site:** [PicoCTF](https://play.picoctf.org/)  
> **Author:** LT 'syreal' Jones  
> **Description:** This service can provide you with a random number, but can it do anything else? Additional details will be available after launching your challenge instance.  
Connect to the program with netcat: $ nc saturn.picoctf.net XXXXX The program's source code can be downloaded here.  
> **Files:** [picker-I.py](https://artifacts.picoctf.net/c/513/picker-I.py)  
> **Points:** 100  
> * *Hint 1: Can you point the program to a function that does something useful for you?*

## Investigation

Create a folder in the relevant directory and download the py file.

```bash
cd reverse_engineering
mkdir picker_i && cd $_
wget https://artifacts.picoctf.net/c/513/picker-I.py
```

Let's look at the source code file before we connect to the instance with netcat to try and work out what the program does. There are two esoteric functions and an exit function we can ignore. The important sections of code are the while loop, **getRandomNumber()** and **win()**.

```python
def getRandomNumber():
  print(4)  # Chosen by fair die roll.
            # Guaranteed to be random.
            # (See XKCD)

def win():
  # This line will not work locally unless you create your own 'flag.txt' in
  #   the same directory as this script
  flag = open('flag.txt', 'r').read()
  #flag = flag[:-1]
  flag = flag.strip()
  str_flag = ''
  for c in flag:
    str_flag += str(hex(ord(c))) + ' '
  print(str_flag)

while(True):
  try:
    print('Try entering "getRandomNumber" without the double quotes...')
    user_input = input('==> ')
    eval(user_input + '()')
  except Exception as e:
    print(e)
```

Ignoring the fact that getRandomNumber() only ever returns 4, the important thing is that the user input can call the function directly. The **eval(user_input + '()')** means when we type getRandomNumber the program appends the () and calls the function. From the code we can see that the win() function returns the flag so let's connect to the instance and try passing **win** and see if the script appends the () and calls the function for us.

```bash
try entering "getRandomNumber" without the double quotes...
==> win
0x70 0x69 0x63 0x6f 0x43 0x54 0x46 0x7b 0x34 0x5f 0x64 0x31 0x34 0x6d 0x30 0x6e 0x64 0x5f 0x31 0x6e 0x5f 0x37 0x68 0x33 0x5f 0x72 0x30 0x75 0x67 0x68 0x5f 0x62 0x35 0x32 0x33 0x62 0x32 0x61 0x31 0x7d
```

Success. We have a series of hex characters. Using [CyberChef](https://cyberchef.org/) to decode we now have the flag. For other methods, refer to the [ascii_numbers](../picoctfgym-ascii_numbers) writeup.

### Flag

> FLAG = picoCTF{4_d14m0nd_1n_7h3_r0ugh_b523b2a1}
{: .prompt-tip }

Mark our folder as solved.

```bash
cd ..
mv picker_i{,_SOLVED}
```