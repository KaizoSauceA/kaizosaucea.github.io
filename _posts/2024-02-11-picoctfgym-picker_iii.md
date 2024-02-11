---
title: PicoGym - Picker III
author: KaizoSauceA
date: 2024-02-11 07:30:00 -0
categories: [CTF,PicoGym]
tags: [picoctf_reverse-engineering,linux,python]
---

Refer to the [first entry](../picoctf2021-obedient_cat) in this series for folder structure and tips.

## Challenge Details

> **Site:** [PicoCTF](https://play.picoctf.org/)  
> **Author:** LT 'syreal' Jones  
> **Description:** Can you figure out how this program works to get the flag?
Additional details will be available after launching your challenge instance..  
Connect to the program with netcat: $ nc saturn.picoctf.net XXXXX The program's source code can be downloaded here. 
> **Files:** [picker-III.py](https://artifacts.picoctf.net/c/524/picker-III.py)  
> **Points:** 100  
> * *Hint 1: Is there any way to modify the function table?*

## Investigation

Create a folder in the relevant directory and download the py file.

```bash
cd reverse_engineering
mkdir picker_iii && cd $_
wget https://artifacts.picoctf.net/c/524/picker-III.py
```

This is a more advanced version of the code we broke in [picker_ii](../picoctfgym-picker_ii)

Let's look at the source code file before we connect to the instance with netcat to try and work out what the program does. The first thing to check is the user input section as with previous examples. We can see there is no way to directly call the win() function anymore.

```python
while(USER_ALIVE):
  choice = input('==> ')
  if( choice == 'quit' or choice == 'exit' or choice == 'q' ):
    USER_ALIVE = False
  elif( choice == 'help' or choice == '?' ):
    help_text()
  elif( choice == 'reset' ):
    reset_table()
  elif( choice == '1' ):
    call_func(0)
  elif( choice == '2' ):
    call_func(1)
  elif( choice == '3' ):
    call_func(2)
  elif( choice == '4' ):
    call_func(3)
  else:
    print('Did not understand "'+choice+'" Have you tried "help"?')
```

The **call_func(n)** function ends with **eval(func_name+'()')** so our goal here is to try and get win into the function table somehow. Let's run the program and get some output to tie up against the source py file.

```bash
nc saturn.picoctf.net 62308
==> help

This program fixes vulnerabilities in its predecessor by limiting what
functions can be called to a table of predefined functions. This still puts
the user in charge, but prevents them from calling undesirable subroutines.

* Enter 'quit' to quit the program.
* Enter 'help' for this text.
* Enter 'reset' to reset the table.
* Enter '1' to execute the first function in the table.
* Enter '2' to execute the second function in the table.
* Enter '3' to execute the third function in the table.
* Enter '4' to execute the fourth function in the table.

Here's the current table:
  
1: print_table
2: read_variable
3: write_variable
4: getRandomNumber
```

### Overwrite a Variable

Looking at the table, the plain function names are present as they simply get appended with () in order to execute. Let's try and overwrite the getRandomNumber entry with win and see if we can get it to execute.

```bash
nc saturn.picoctf.net 62308
# we choose write_variable
==> 3
# enter exact name of the existing function
Please enter variable name to write: getRandomNumber
# enter exact name of the win function without the ()
Please enter new value of variable: win
# execute
==> 4
0x70 0x69 0x63 0x6f 0x43 0x54 0x46 0x7b 0x37 0x68 0x31 0x35 0x5f 0x31 0x35 0x5f 0x77 0x68 0x34 0x37 0x5f 0x77 0x33 0x5f 0x67 0x33 0x37 0x5f 0x77 0x31 0x37 0x68 0x5f 0x75 0x35 0x33 0x72 0x35 0x5f 0x31 0x6e 0x5f 0x63 0x68 0x34 0x72 0x67 0x33 0x5f 0x63 0x32 0x30 0x66 0x35 0x32 0x32 0x32 0x7d
```

We have a series of hex characters. Using [CyberChef](https://cyberchef.org/) to decode we now have the flag. For other methods, refer to the [ascii_numbers](../picoctfgym-ascii_numbers) writeup.

### Breaking the Table

There is a check in the code for the length of the func_table. We could try simply overwriting the entire thing with win and buffering in extra characters so as to still pass the len() logic check.

```python
USER_ALIVE = True
FUNC_TABLE_SIZE = 4
FUNC_TABLE_ENTRY_SIZE = 32
CORRUPT_MESSAGE = 'Table corrupted. Try entering \'reset\' to fix it'

func_table = ''

def reset_table():
  global func_table

  # This table is formatted for easier viewing, but it is really one line
  func_table = \
'''\
print_table                     \
read_variable                   \
write_variable                  \
getRandomNumber                 \
'''

def check_table():
  global func_table

  if( len(func_table) != FUNC_TABLE_ENTRY_SIZE * FUNC_TABLE_SIZE):
    return False

  return True
```

We need to get win along with a space after it (in order to be identified as an individual entry) and then ensure we meet the table length requirement.

```bash
# FUNC_TABLE_SIZE = 4
# FUNC_TABLE_ENTRY_SIZE = 32
# 4 * 32 = 128
# "win" + " " = 4
# make sure we subtract the length of "win" + " " from the total so the payload doesn't trigger the corrupted table check
python
>>> "win" + " " + ("x" * 124)
'win xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx'
# this is our generated payload. Reconnect
nc saturn.picoctf.net 62308
# choose the write_variable
==> 3
# this time we overwrite the func_table itself
Please enter variable name to write: func_table
# paste our payload as the new entry
Please enter new value of variable: 'win xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx'
# print the table and it has been overwritten to call win
==> 1
0x70 0x69 0x63 0x6f 0x43 0x54 0x46 0x7b 0x37 0x68 0x31 0x35 0x5f 0x31 0x35 0x5f 0x77 0x68 0x34 0x37 0x5f 0x77 0x33 0x5f 0x67 0x33 0x37 0x5f 0x77 0x31 0x37 0x68 0x5f 0x75 0x35 0x33 0x72 0x35 0x5f 0x31 0x6e 0x5f 0x63 0x68 0x34 0x72 0x67 0x33 0x5f 0x63 0x32 0x30 0x66 0x35 0x32 0x32 0x32 0x7d
```

### Flag

> FLAG = picoCTF{7h15_15_wh47_w3_g37_w17h_u53r5_1n_ch4rg3_c20f5222}
{: .prompt-tip }

Mark our folder as solved.

```bash
cd ..
mv picker_iii{,_SOLVED}
```