---
title: PicoCTF 2021 - Obedient Cat
author: KaizoSauceA
date: 2024-02-09 06:00:00 -0
categories: [CTF,PicoCTF 2021]
tags: [picoctf_general-skills,linux]
---

## Challenge Details

> **Site:** [PicoCTF](https://play.picoctf.org/)  
> **Author:** syreal  
> **Description:** This file has a flag in plain sight (aka "in-the-clear").  
> **Files:** [Download flag](https://mercury.picoctf.net/static/217686fc11d733b80be62dcfcfca6c75/flag)  
> **Points:** 5  
> * *Hint 1: Any hints about entering a command into the Terminal (such as the next one), will start with a '$'... everything after the dollar sign will be typed (or copy and pasted) into your Terminal.*
> * *Hint 2: To get the file accessible in your shell, enter the following in the Terminal prompt: $ wget https://mercury.picoctf.net/static/217686fc11d733b80be62dcfcfca6c75/flag*
> * *Hint 3: $ man cat*

## First Steps

I have set up my folder structure to align with the categories of PicoCTF for easier sorting.

```bash
cd PicoCTF
tree               
.
├── binary_exploitation
├── cryptography
├── forensics
├── general_skills
├── reverse_engineering
├── uncategorized
└── web_exploitation
```

## Investigation

This is a simple entry level CTF to get us started. 
Let's create a folder in the relevant directory and download the flag file using **wget** as instructed in the hints.

```bash
cd general_skills
# create new directory AND cd into it
mkdir obedient_cat && cd $_
wget https://mercury.picoctf.net/static/217686fc11d733b80be62dcfcfca6c75/flag
dir
flag
```

Inspecting the file tell's us it is just simple text. So let's use **cat** to print the contents.

```bash
file flag   
flag: ASCII text
# use 'man cat' from hint 3 to read the cat manual
cat flag                 
picoCTF{s4n1ty_v3r1f13d_b5aeb3dd}
```

And just like that, we have found our very first flag. Copy and paste into our PicoCTF dashboard and we are on our way!

### Flag

> FLAG = picoCTF{s4n1ty_v3r1f13d_b5aeb3dd}
{: .prompt-tip }

For best practice let's mark our folder as solved.

```bash
cd ..
# rename in line with one command appending _SOLVED
mv obedient_cat{,_SOLVED}
dir
obedient_cat_SOLVED
```