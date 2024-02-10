---
title: PicoGym - Big Zip
author: KaizoSauceA
date: 2024-02-10 13:20:00 -0
categories: [CTF,PicoGym]
tags: [picoctf_general-skills,linux]
---

Refer to the [first entry](../picoctf2021-obedient_cat) in this series for folder structure and tips.

## Challenge Details

> **Site:** [PicoCTF](https://play.picoctf.org/)  
> **Author:** LT 'syreal' Jones  
> **Description:** Unzip this archive and find the file the flag'   
> **Files:** [big-zip-files.zip](https://artifacts.picoctf.net/c/504/big-zip-files.zip)  
> **Points:** 100  
> * *Hint 1: Can grep be instructed to look at every file in a directory and its subdirectories?*

## Investigation

Create a folder in the relevant directory, download the zip file and extract.

```bash
cd general_skills
mkdir big_zip && cd $_
wget https://artifacts.picoctf.net/c/504/big-zip-files.zip
unzip big-zip-files.zip
```

We need to use grep as per the hint, passing the **-r** argument for recursion and searching for the "CTF" string we assume to be part of the flag.

```bash
grep -r 'CTF' ./       
./big-zip-files/folder_pmbymkjcya/folder_cawigcwvgv/folder_ltdayfmktr/folder_fnpfclfyee/whzxrpivpqld.txt:information on the record will last a billion years. Genes and brains and books encode picoCTF{gr3p_15_m4g1c_ef8790dc}
```

### Flag

> FLAG = picoCTF{gr3p_15_m4g1c_ef8790dc}
{: .prompt-tip }

Mark our folder as solved.

```bash
cd ..
mv big_zip{,_SOLVED}
```