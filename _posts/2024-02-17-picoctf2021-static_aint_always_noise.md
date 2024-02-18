---
title: PicoCTF 2021 - Static Ain't Always Noise
author: KaizoSauceA
date: 2024-02-17 11:40:00 -0
categories: [CTF,PicoCTF 2021]
tags: [picoctf_general-skills,linux]
---

Refer to the [first entry](../picoctf2021-obedient_cat) in this series for folder structure and tips.

## Challenge Details

> **Site:** [PicoCTF](https://play.picoctf.org/)  
> **Author:** syreal  
> **Description:** Can you look at the data in this binary: static? This BASH script might help!  
> **Files:** [static](https://mercury.picoctf.net/static/bc72945175d643626d6ea9a689672dbd/static) [BASH Script](https://mercury.picoctf.net/static/bc72945175d643626d6ea9a689672dbd/ltdis.sh)  
> **Points:** 20  
> * *No Hints*

## Investigation

Create a folder in the relevant directory, download the files.

```bash
cd general_skills
mkdir static_aint_always_noise && cd $_
wget https://mercury.picoctf.net/static/bc72945175d643626d6ea9a689672dbd/static
wget https://mercury.picoctf.net/static/bc72945175d643626d6ea9a689672dbd/ltdis.sh
```

Let's run the bash script we are told that we must pass the program file.

```bash
bash ./ltdis.sh 
Attempting disassembly of  ...
objdump: 'a.out': No such file
objdump: section '.text' mentioned in a -j option, but not found in any input file
Disassembly failed!
Usage: ltdis.sh <program-file>
Bye!
```

Let's run again with the correct syntax.

```bash
bash ./ltdis.sh static
Attempting disassembly of static ...
Disassembly successful! Available at: static.ltdis.x86_64.txt
Ripping strings from binary with file offsets...
Any strings found in static have been written to static.ltdis.strings.txt with file offset
```

The script has generated two new files. Let's grep on our directory and see if the flag is in plain text somewhere in the files.

```bash
dir                         
ltdis.sh  static  static.ltdis.strings.txt  static.ltdis.x86_64.txt

grep -r 'CTF' ./
./static.ltdis.strings.txt:   1020 picoCTF{d15a5m_t34s3r_1e6a7731}
grep: ./static: binary file matches
```

### Flag

> FLAG = picoCTF{d15a5m_t34s3r_1e6a7731}
{: .prompt-tip }

Mark our folder as solved.

```bash
cd ..
mv static_aint_always_noise{,_SOLVED}
```