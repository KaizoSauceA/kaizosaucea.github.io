---
title: PicoGym - First Find
author: KaizoSauceA
date: 2024-02-10 12:30:00 -0
categories: [CTF,PicoGym]
tags: [picoctf_general-skills,linux]
---

Refer to the [first entry](../picoctf2021-obedient_cat) in this series for folder structure and tips.

## Challenge Details

> **Site:** [PicoCTF](https://play.picoctf.org/)  
> **Author:** LT 'syreal' Jones  
> **Description:** Unzip this archive and find the file named 'uber-secret.txt'   
> **Files:** [files.zip](https://artifacts.picoctf.net/c/502/files.zip)  
> **Points:** 100  
> * *No Hints*

## Investigation

Create a folder in the relevant directory, download the zip file and extract. We can actually cheat right here and see where the **uber-secret.txt** file is.

```bash
cd general_skills
mkdir first_find && cd $_
wget https://artifacts.picoctf.net/c/502/files.zip
unzip files.zip        
Archive:  files.zip
   creating: files/
   creating: files/satisfactory_books/
   creating: files/satisfactory_books/more_books/
  inflating: files/satisfactory_books/more_books/37121.txt.utf-8  
  inflating: files/satisfactory_books/23765.txt.utf-8  
  inflating: files/satisfactory_books/16021.txt.utf-8  
  inflating: files/13771.txt.utf-8   
   creating: files/adequate_books/
   creating: files/adequate_books/more_books/
   creating: files/adequate_books/more_books/.secret/
   creating: files/adequate_books/more_books/.secret/deeper_secrets/
   creating: files/adequate_books/more_books/.secret/deeper_secrets/deepest_secrets/
 extracting: files/adequate_books/more_books/.secret/deeper_secrets/deepest_secrets/uber-secret.txt  
  inflating: files/adequate_books/more_books/1023.txt.utf-8  
  inflating: files/adequate_books/46804-0.txt  
  inflating: files/adequate_books/44578.txt.utf-8  
   creating: files/acceptable_books/
   creating: files/acceptable_books/more_books/
  inflating: files/acceptable_books/more_books/40723.txt.utf-8  
  inflating: files/acceptable_books/17880.txt.utf-8  
  inflating: files/acceptable_books/17879.txt.utf-8  
  inflating: files/14789.txt.utf-8
```

### Tree

We can run **tree** with the **-a** argument to show all folders also.

```bash
tree -a            
.
├── files
│   ├── 13771.txt.utf-8
│   ├── 14789.txt.utf-8
│   ├── acceptable_books
│   │   ├── 17879.txt.utf-8
│   │   ├── 17880.txt.utf-8
│   │   └── more_books
│   │       └── 40723.txt.utf-8
│   ├── adequate_books
│   │   ├── 44578.txt.utf-8
│   │   ├── 46804-0.txt
│   │   └── more_books
│   │       ├── 1023.txt.utf-8
│   │       └── .secret
│   │           └── deeper_secrets
│   │               └── deepest_secrets
│   │                   └── uber-secret.txt
│   └── satisfactory_books
│       ├── 16021.txt.utf-8
│       ├── 23765.txt.utf-8
│       └── more_books
│           └── 37121.txt.utf-8
└── files.zip

cat files/adequate_books/more_books/.secret/deeper_secrets/deepest_secrets/uber-secret.txt
picoCTF{f1nd_15_f457_ab443fd1}
```

### Grep

We can also assume the flag will have "CTF" as part of the string so we can use grep

```bash
grep -r 'CTF'   
grep: files.zip: binary file matches
files/adequate_books/more_books/.secret/deeper_secrets/deepest_secrets/uber-secret.txt:picoCTF{f1nd_15_f457_ab443fd1}
```

### Flag

> FLAG = picoCTF{f1nd_15_f457_ab443fd1}
{: .prompt-tip }

Mark our folder as solved.

```bash
cd ..
mv first_find{,_SOLVED}
```