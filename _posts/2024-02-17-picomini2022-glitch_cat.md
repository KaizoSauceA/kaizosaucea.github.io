---
title: PicoMini 2022 - Glitch Cat
author: KaizoSauceA
date: 2024-02-17 08:30:00 -0
categories: [CTF,PicoMini 2022]
tags: [picoctf_general-skills,linux,python]
---

Refer to the [first entry](../picoctf2021-obedient_cat) in this series for folder structure and tips.

## Challenge Details

> **Site:** [PicoCTF](https://play.picoctf.org/)  
> **Author:** LT 'syreal' Jones  
> **Description:** Our flag printing service has started glitching! $ nc saturn.picoctf.net 52682  
> **Files:** N/A  
> **Points:** 100  
> * *Hint 1: ASCII is one of the most common encodings used in programming.*  
> * *Hint 2: We know that the glitch output is valid Python, somehow!*  
> * *Hint 3: Press Ctrl and c on your keyboard to close your connection and return to the command prompt.*

## Investigation

There are no files to download. Let's connect using netcat and see what we get.

```bash
nc saturn.picoctf.net 52682
'picoCTF{gl17ch_m3_n07_' + chr(0x62) + chr(0x64) + chr(0x61) + chr(0x36) + chr(0x38) + chr(0x66) + chr(0x37) + chr(0x35) + '}'
```

If we copy the above, open python and paste it in python should print out the flag in plain text.

```python
python
>>> 'picoCTF{gl17ch_m3_n07_' + chr(0x62) + chr(0x64) + chr(0x61) + chr(0x36) + chr(0x38) + chr(0x66) + chr(0x37) + chr(0x35) + '}'
'picoCTF{gl17ch_m3_n07_bda68f75}'
```

### Flag

> FLAG = picoCTF{gl17ch_m3_n07_bda68f75}
{: .prompt-tip }