---
title: PicoMini 2022 - HashingJobApp
author: KaizoSauceA
date: 2024-02-17 08:40:00 -0
categories: [CTF,PicoMini 2022]
tags: [picoctf_general-skills,linux]
---

Refer to the [first entry](../picoctf2021-obedient_cat) in this series for folder structure and tips.

## Challenge Details

> **Site:** [PicoCTF](https://play.picoctf.org/)  
> **Author:** LT 'syreal' Jones  
> **Description:** If you want to hash with the best, beat this test! nc saturn.picoctf.net 53638  
> **Files:** N/A  
> **Points:** 100  
> * *Hint 1: You can use a command line tool or web app to hash text.*  
> * *Hint 2: Press Ctrl and c on your keyboard to close your connection and return to the command prompt.*

## Investigation

There are no files to download. Let's connect using netcat and see what we get.

```bash
nc saturn.picoctf.net 53638
Please md5 hash the text between quotes, excluding the quotes: 'morticians'
Answer:
```

If we use the the generator at [MD5 Hash Generator](https://www.md5hashgenerator.com/), paste in **morticians**, we get our hash of 'f6dfb72af8c3e3397851094d7f1a8605'. Paste that in to the tool. We get prompted two more times for hashes of different words. Repeat the steps and eventually we get the flag.

```bash
nc saturn.picoctf.net 53638
Please md5 hash the text between quotes, excluding the quotes: 'morticians'
Answer: 
f6dfb72af8c3e3397851094d7f1a8605
Correct.
Please md5 hash the text between quotes, excluding the quotes: 'cholesterol'
Answer: 
6b2aa5ed34cf2177d4b5c831f040cd0c
Correct.
Please md5 hash the text between quotes, excluding the quotes: 'Hollywood'
Answer: 
1ac441036f927b9815ba1137464ee064
Correct.
picoCTF{4ppl1c4710n_r3c31v3d_bf2ceb02}
```

### Flag

> FLAG = picoCTF{4ppl1c4710n_r3c31v3d_bf2ceb02}
{: .prompt-tip }