---
title: PicoCTF 2022 - Bloat.py
author: KaizoSauceA
date: 2024-02-13 08:30:00 -0
categories: [CTF,PicoCTF 2022]
tags: [picoctf_reverse_engineering,linux,python]
---

Refer to the [first entry](../picoctf2021-obedient_cat) in this series for folder structure and tips.

## Challenge Details

> **Site:** [PicoCTF](https://play.picoctf.org/)  
> **Author:** LT 'syreal' Jones  
> **Description:** Run this Python program in the same directory as this encrypted flag.   
> **Files:** [bloat.flag.py](https://artifacts.picoctf.net/c/104/bloat.flag.py) [flag.txt.enc](https://artifacts.picoctf.net/c/104/flag.txt.enc)  
> **Points:** 200  
> * *No Hints*  

## Investigation

Create a folder in the relevant directory, download the py file and flag file.

```bash
cd reverse_engineering
mkdir bloat_py && cd $_
wget https://artifacts.picoctf.net/c/104/bloat.flag.py
wget https://artifacts.picoctf.net/c/104/flag.txt.enc
```

Let's run the python file. We are prompted for the password which we don't have.

```python
python bloat.flag.py   
Please enter correct password for flag: 1234
That password is incorrect
```

So let's make a copy of the py file called tinker.py and open it in a code editor. On first inspection the file doesn't make much sense with no friendly function names or strings.

```python
import sys

a = "!\"#$%&'()*+,-./0123456789:;<=>?@ABCDEFGHIJKLMNOPQRSTUVWXYZ"+ \
            "[\\]^_`abcdefghijklmnopqrstuvwxyz{|}~ "

def arg133(arg432):
  if arg432 == a[71]+a[64]+a[79]+a[79]+a[88]+a[66]+a[71]+a[64]+a[77]+a[66]+a[68]:
    return True
  else:
    print(a[51]+a[71]+a[64]+a[83]+a[94]+a[79]+a[64]+a[82]+a[82]+a[86]+a[78]+\
a[81]+a[67]+a[94]+a[72]+a[82]+a[94]+a[72]+a[77]+a[66]+a[78]+a[81]+\
a[81]+a[68]+a[66]+a[83])
    sys.exit(0)
    return False
def arg111(arg444):
  return arg122(arg444.decode(), a[81]+a[64]+a[79]+a[82]+a[66]+a[64]+a[75]+\
a[75]+a[72]+a[78]+a[77])
```

### Cleaning Up the Code

The sample above shows variable **a** consisting of string of characters, numbers and the alphabet in both cases. Then the if and print statements used within functions reference indexes of that variable. We can open python from the command line, configure variable a and then paste in the indexing to reverse engineer the strings.

```bash
python
>>> a = "!\"#$%&'()*+,-./0123456789:;<=>?@ABCDEFGHIJKLMNOPQRSTUVWXYZ"+ \
...             "[\\]^_`abcdefghijklmnopqrstuvwxyz{|}~ "
>>> a[71]+a[64]+a[79]+a[79]+a[88]+a[66]+a[71]+a[64]+a[77]+a[66]+a[68]
'happychance'
>>> a[51]+a[71]+a[64]+a[83]+a[94]+a[79]+a[64]+a[82]+a[82]+a[86]+a[78]+\
... a[81]+a[67]+a[94]+a[72]+a[82]+a[94]+a[72]+a[77]+a[66]+a[78]+a[81]+\
... a[81]+a[68]+a[66]+a[83]
'That password is incorrect'
>>> a[81]+a[64]+a[79]+a[82]+a[66]+a[64]+a[75]+\
... a[75]+a[72]+a[78]+a[77]
'rapscallion'
>>> a[47]+a[75]+a[68]+a[64]+a[82]+a[68]+a[94]+a[68]+a[77]+a[83]+\
... a[68]+a[81]+a[94]+a[66]+a[78]+a[81]+a[81]+a[68]+a[66]+a[83]+\
... a[94]+a[79]+a[64]+a[82]+a[82]+a[86]+a[78]+a[81]+a[67]+a[94]+\
... a[69]+a[78]+a[81]+a[94]+a[69]+a[75]+a[64]+a[70]+a[25]+a[94]
'Please enter correct password for flag: '
```

If we do the above for all of the indexes, and then replace them in the tinker.py file our code is far more readable. We can also infer the purpose of the functions and variables. We can give them our own names to make things even more readable. For example **def arg133(arg432)** is clearly the checking that the user input matches a password or not. We don't need to go much further in this case unless we want to reverse everything. We can be sure the password is 'happychance'.

```python
import sys

a = "!\"#$%&'()*+,-./0123456789:;<=>?@ABCDEFGHIJKLMNOPQRSTUVWXYZ"+ \
            "[\\]^_`abcdefghijklmnopqrstuvwxyz{|}~ "

def arg133(arg432):
  if arg432 == "happychance":
    return True
  else:
    print("That password is incorrect")
    sys.exit(0)
    return False

def arg111(arg444):
  return arg122(arg444.decode(), "rapscallion")

def arg232():
  return input("Please enter correct password for flag: ")

def arg132():
  return open('flag.txt.enc', 'rb').read()

def arg112():
  print("Welcome back... your flag, user:")

def arg122(arg432, arg423):
    arg433 = arg423
    i = 0
    while len(arg433) < len(arg432):
        arg433 = arg433 + arg423[i]
        i = (i + 1) % len(arg423)        
    return "".join([chr(ord(arg422) ^ ord(arg442)) for (arg422,arg442) in zip(arg432,arg433)])

arg444 = arg132()
arg432 = arg232()

arg133(arg432)
arg112()
arg423 = arg111(arg444)
print(arg423)
sys.exit(0)
```

Let's run the original file and pass in the password. We have the flag.

```bash
python bloat.flag.py 
Please enter correct password for flag: happychance
Welcome back... your flag, user:
picoCTF{d30bfu5c4710n_f7w_161a4f09}
```

### Flag

> FLAG = picoCTF{d30bfu5c4710n_f7w_161a4f09}
{: .prompt-tip }

Mark our folder as solved.

```bash
cd ..
mv bloat_py{,_SOLVED}
```