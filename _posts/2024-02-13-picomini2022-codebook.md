---
title: PicoMini 2022 - Codebook
author: KaizoSauceA
date: 2024-02-13 15:10:00 -0
categories: [CTF,PicoMini 2022]
tags: [picoctf_general-skills,linux,python]
---

Refer to the [first entry](../picoctf2021-obedient_cat) in this series for folder structure and tips.

## Challenge Details

> **Site:** [PicoCTF](https://play.picoctf.org/)  
> **Author:** LT 'syreal' Jones  
> **Description:** Run the Python script code.py in the same directory as codebook.txt.
> **Files:** [code.py](https://artifacts.picoctf.net/c/3/code.py) [codebook.txt](https://artifacts.picoctf.net/c/3/codebook.txt)  
> **Points:** 100  
> * *Hint 1: On the webshell, use ls to see if both files are in the directory you are in.*  
> * *Hint 2: The str_xor function does not need to be reverse engineered for this challenge.*

## Investigation

Create a folder in the relevant directory, download all the files.

```bash
cd general_skills
mkdir codebook && cd $_
wget https://artifacts.picoctf.net/c/3/code.py
wget https://artifacts.picoctf.net/c/3/codebook.txt
```

Run the python file. Flag obtained. This is a simple test to ensure files are in the same directory.

```bash
python code.py                                     
picoCTF{c0d3b00k_455157_197a982c}
```

### Flag

> FLAG = picoCTF{c0d3b00k_455157_197a982c}
{: .prompt-tip }

Mark our folder as solved.

```bash
cd ..
mv codebook{,_SOLVED}
```