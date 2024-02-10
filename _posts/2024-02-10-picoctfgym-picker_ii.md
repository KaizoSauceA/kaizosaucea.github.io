---
title: PicoGym - Picker II
author: KaizoSauceA
date: 2024-02-10 14:00:00 -0
categories: [CTF,PicoGym]
tags: [picoctf_reverse-engineering,linux,python]
---

Refer to the [first entry](../picoctf2021-obedient_cat) in this series for folder structure and tips.

## Challenge Details

> **Site:** [PicoCTF](https://play.picoctf.org/)  
> **Author:** LT 'syreal' Jones  
> **Description:** This service can provide you with a random number, but can it do anything else? Additional details will be available after launching your challenge instance.  
Connect to the program with netcat: $ nc saturn.picoctf.net XXXXX The program's source code can be downloaded here.  
> **Files:** [picker-II.py](https://artifacts.picoctf.net/c/521/picker-II.py)  
> **Points:** 100  
> * *Hint 1: Can you do what win does with your input to the program?*

## Investigation

Create a folder in the relevant directory and download the py file.

```bash
cd reverse_engineering
mkdir picker_ii && cd $_
wget https://artifacts.picoctf.net/c/521/picker-II.py
```

This is a more advanced version of the code we broke in [picker_i](../picogym-picker_i)

Let's look at the source code file before we connect to the instance with netcat to try and work out what the program does. It is almost the same, but with the addition of a filter to check our input to prohibit us from directly inputting **win** to call the function for the flag.

```python
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

def filter(user_input):
  if 'win' in user_input:
    return False
  return True

while(True):
  try:
    user_input = input('==> ')
    if( filter(user_input) ):
      eval(user_input + '()')
    else:
      print('Illegal input')
  except Exception as e:
    print(e)
```

### Bypass the Filter

The program itself is using the **eval()** function to join our input with the () so why don't we use the same logic to wrap our call to win and try to bypass the filter.

```bash
nc saturn.picoctf.net 62753
==> eval('w' + 'i' + 'n')
0x70 0x69 0x63 0x6f 0x43 0x54 0x46 0x7b 0x66 0x31 0x6c 0x37 0x33 0x72 0x35 0x5f 0x66 0x34 0x31 0x6c 0x5f 0x63 0x30 0x64 0x33 0x5f 0x72 0x33 0x66 0x34 0x63 0x37 0x30 0x72 0x5f 0x6d 0x31 0x67 0x68 0x37 0x5f 0x35 0x75 0x63 0x63 0x33 0x33 0x64 0x5f 0x62 0x39 0x32 0x34 0x65 0x38 0x65 0x35 0x7d
```

It worked. The eval function was passed to the filter function but returned True because it didn't match the string "win"; after which it was evaluated (concatenated in this case) to win, allowing us to call the win() function.

We have a series of hex characters. Using [CyberChef](https://cyberchef.org/) to decode we now have the flag. For other methods, refer to the [ascii_numbers](../picoctfgym-ascii_numbers) writeup.

### Bypass Encoding

As we know the file name of the flag from the source code and that the program accepts python syntax (only filtering for 'win') we could also just open the flag, read and print it directly. This avoids the win() function and the builtin encoding and gives us the flag immediately.

```bash
nc saturn.picoctf.net 62753
==> print(open('flag.txt').read())
picoCTF{f1l73r5_f41l_c0d3_r3f4c70r_m1gh7_5ucc33d_b924e8e5}
'NoneType' object is not callable
```

### Flag

> FLAG = picoCTF{f1l73r5_f41l_c0d3_r3f4c70r_m1gh7_5ucc33d_b924e8e5}
{: .prompt-tip }

Mark our folder as solved.

```bash
cd ..
mv picker_ii{,_SOLVED}
```