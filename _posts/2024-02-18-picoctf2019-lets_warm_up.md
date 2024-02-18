---
title: PicoCTF 2019 - Let's Warm Up
author: KaizoSauceA
date: 2024-02-18 07:50:00 -0
categories: [CTF,PicoCTF 2019]
tags: [picoctf_general-skills,linux]
---

Refer to the [first entry](../picoctf2021-obedient_cat) in this series for folder structure and tips.

## Challenge Details

> **Site:** [PicoCTF](https://play.picoctf.org/)  
> **Author:** Sanjay C/Danny Tunitis  
> **Description:** If I told you a word started with 0x70 in hexadecimal, what would it start with in ASCII?  
> **Files:** N/A  
> **Points:** 50  
> * *Hint 1: Submit your answer in our flag format. For example, if your answer was 'hello', you would submit 'picoCTF{hello}' as the flag.*

## Investigation

Let's use the online calculator at [RapidTables](https://www.rapidtables.com/convert/number/hex-to-ascii.html) and convert 0x70 from hex to ascii. The answer is **p**. If we wrap that in the picoCTF format we have the flag. Simple.

### Flag

> FLAG = picoCTF{p}
{: .prompt-tip }