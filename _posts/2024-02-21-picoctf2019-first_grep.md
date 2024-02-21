---
title: PicoCTF 2019 - First Grep
author: KaizoSauceA
date: 2024-02-21 12:40:00 -0
categories: [CTF,PicoCTF 2019]
tags: [picoctf_general-skills,linux]
---

Refer to the [first entry](../picoctf2021-obedient_cat) in this series for folder structure and tips.

## Challenge Details

> **Site:** [PicoCTF](https://play.picoctf.org/)  
> **Author:** Alex Fulton/Danny Tunitis  
> **Description:** Can you find the flag in file? This would be really tedious to look through manually, something tells me there is a better way.  
> **Files:** [file](https://jupiter.challenges.picoctf.org/static/515f19f3612bfd97cd3f0c0ba32bd864/file)  
> **Points:** 100  
> * *Hint 1: grep [tutorial](https://ryanstutorials.net/linuxtutorial/grep.php)*

## Investigation

Create a folder in the relevant directory and download the file.

```bash
cd general_skills
mkdir first_grep && cd $_
wget https://jupiter.challenges.picoctf.org/static/515f19f3612bfd97cd3f0c0ba32bd864/file
```

If we run **file** on the file we see it is a text file. Let's cat the file and grep for "CTF" which we expect to be in the flag.

```bash
file file                 
file: ASCII text, with very long lines (4200)

cat file | grep CTF
picoCTF{grep_is_good_to_find_things_5af9d829}
```

### Flag

> FLAG = picoCTF{grep_is_good_to_find_things_5af9d829}
{: .prompt-tip }

Mark our folder as solved.

```bash
cd ..
mv first_grep{,_SOLVED}
```