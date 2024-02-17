---
title: PicoMini 2022 - Serpentine
author: KaizoSauceA
date: 2024-02-17 09:00:00 -0
categories: [CTF,PicoMini 2022]
tags: [picoctf_general-skills,linux,python]
---

Refer to the [first entry](../picoctf2021-obedient_cat) in this series for folder structure and tips.

## Challenge Details

> **Site:** [PicoCTF](https://play.picoctf.org/)  
> **Author:** LT 'syreal' Jones  
> **Description:** Find the flag in the Python script! Download Python script.   
> **Files:** [serpentine.py](https://artifacts.picoctf.net/c/35/serpentine.py)  
> **Points:** 100  
> * *Hint 1: Try running the script and see what happens.*  
> * *Hint 2: In the webshell, try examining the script with a text editor like nano.*  
> * *Hint 3: To exit nano, press Ctrl and x and follow the on-screen prompts.*
> * *Hint 4: The str_xor function does not need to be reverse engineered for this challenge.*

## Investigation

Create a folder in the relevant directory, download the file.

```bash
cd general_skills
mkdir serpentine && cd $_
wget https://artifacts.picoctf.net/c/35/serpentine.py
```

Let's run the python file. When we choose the option to print the flag we get a message that the flag has been misplaced and told to look at the source code.

```bash
python serpentine.py                                                                                                                 

    Y
  .-^-.
 /     \      .- ~ ~ -.
()     ()    /   _ _   `.                     _ _ _
 \_   _/    /  /     \   \                . ~  _ _  ~ .
   | |     /  /       \   \             .' .~       ~-. `.
   | |    /  /         )   )           /  /             `.`.
   \ \_ _/  /         /   /           /  /                `'
    \_ _ _.'         /   /           (  (
                    /   /             \  \
                   /   /               \  \
                  /   /                 )  )
                 (   (                 /  /
                  `.  `.             .'  /
                    `.   ~ - - - - ~   .'
                       ~ . _ _ _ _ . ~

Welcome to the serpentine encourager!


a) Print encouragement
b) Print flag
c) Quit

What would you like to do? (a/b/c) b

Oops! I must have misplaced the print_flag function! Check my source code!
```

Let's create copy of the file called tinker.py and investigate.

```python
while True:
    print('a) Print encouragement')
    print('b) Print flag')
    print('c) Quit\n')
    choice = input('What would you like to do? (a/b/c) ')
    
    if choice == 'a':
      print_encouragement()
      
    elif choice == 'b':
      print('\nOops! I must have misplaced the print_flag function! Check my source code!\n\n')
      
    elif choice == 'c':
      sys.exit(0)
```

The issue is that the **b** choice prints a the error string. Let's change that choice to the **print_flag()** function. Save the file and run tinker.py.

```python
    elif choice == 'b':
      print_flag()
```

```bash
Welcome to the serpentine encourager!


a) Print encouragement
b) Print flag
c) Quit

What would you like to do? (a/b/c) b
picoCTF{7h3_r04d_l355_7r4v3l3d_ae0b80bd}
```

### Flag

> FLAG = picoCTF{7h3_r04d_l355_7r4v3l3d_ae0b80bd}
{: .prompt-tip }

Mark our folder as solved.

```bash
cd ..
mv serpentine{,_SOLVED}
```