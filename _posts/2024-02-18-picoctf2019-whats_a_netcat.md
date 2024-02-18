---
title: PicoCTF 2019 - What's a Netcat?
author: KaizoSauceA
date: 2024-02-18 08:10:00 -0
categories: [CTF,PicoCTF 2019]
tags: [picoctf_general-skills,linux]
---

Refer to the [first entry](../picoctf2021-obedient_cat) in this series for folder structure and tips.

## Challenge Details

> **Site:** [PicoCTF](https://play.picoctf.org/)  
> **Author:** Sanjay C/Danny Tunitis  
> **Description:** Using netcat (nc) is going to be pretty important. Can you connect to jupiter.challenges.picoctf.org at port 41120 to get the flag?  
> **Files:** N/A  
> **Points:** 100  
> * *Hint 1: nc [tutorial](https://linux.die.net/man/1/nc)*

## Investigation

Let's connect to the server and see what happens. We get the flag. Simple challenge to learn basic net cat functionality.

```bash
nc jupiter.challenges.picoctf.org 41120
You're on your way to becoming the net cat master
picoCTF{nEtCat_Mast3ry_3214be47}
```

### Flag

> FLAG = picoCTF{nEtCat_Mast3ry_3214be47}
{: .prompt-tip }