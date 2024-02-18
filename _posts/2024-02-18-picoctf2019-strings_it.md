---
title: PicoCTF 2019 - Strings It
author: KaizoSauceA
date: 2024-02-18 08:15:00 -0
categories: [CTF,PicoCTF 2019]
tags: [picoctf_general-skills,linux]
---

Refer to the [first entry](../picoctf2021-obedient_cat) in this series for folder structure and tips.

## Challenge Details

> **Site:** [PicoCTF](https://play.picoctf.org/)  
> **Author:** Sanjay C/Danny Tunitis  
> **Description:** Can you find the flag in file without running it?  
> **Files:** [strings](https://jupiter.challenges.picoctf.org/static/fae9ac5267cd6e44124e559b901df177/strings)  
> **Points:** 100  
> * *Hint 1: [strings](https://linux.die.net/man/1/strings)*

## Investigation

Create a folder in the relevant directory, download the zip file.

```bash
cd general_skills
mkdir strings_it && cd $_
wget https://jupiter.challenges.picoctf.org/static/fae9ac5267cd6e44124e559b901df177/strings
```

Let's use strings on the file and grep for 'CTF' which we expect to be part of the flag.

```bash
strings strings | grep CTF
YzOejwCTF3GVzbdb8PkOKp1cKvAwEUvRSOLLm1yFFETiT
picoCTF{5tRIng5_1T_7f766a23}
7Oqu9T7p8SAoQcOcQVHM46k1xpt1M6Iu2ag4dw1OFCTFRbv6
```

### Flag

> FLAG = picoCTF{5tRIng5_1T_7f766a23}
{: .prompt-tip }

Mark our folder as solved.

```bash
cd ..
mv strings_it{,_SOLVED}
```