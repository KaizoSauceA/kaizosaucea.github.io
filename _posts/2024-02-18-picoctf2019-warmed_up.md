---
title: PicoCTF 2019 - Warmed Up
author: KaizoSauceA
date: 2024-02-18 08:00:00 -0
categories: [CTF,PicoCTF 2019]
tags: [picoctf_general-skills,linux]
---

Refer to the [first entry](../picoctf2021-obedient_cat) in this series for folder structure and tips.

## Challenge Details

> **Site:** [PicoCTF](https://play.picoctf.org/)  
> **Author:** Sanjay C/Danny Tunitis  
> **Description:** What is 0x3D (base 16) in decimal (base 10)?  
> **Files:** N/A  
> **Points:** 50  
> * *Hint 1: Submit your answer in our flag format. For example, if your answer was '22', you would submit 'picoCTF{22}' as the flag.*

## Investigation

Let's use the online calculator at [RapidTables](https://www.rapidtables.com/convert/number/base-converter.html) and convert 0x3D from base16 to base10. The answer is **61**. If we wrap that in the picoCTF format we have the flag. Simple.

### Flag

> FLAG = picoCTF{61}
{: .prompt-tip }