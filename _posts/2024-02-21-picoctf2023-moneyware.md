---
title: PicoCTF 2023 - Money-Ware
author: KaizoSauceA
date: 2024-02-21 14:40:00 -0
categories: [CTF,PicoCTF 2023]
tags: [picoctf_general-skills,linux]
---

Refer to the [first entry](../picoctf2021-obedient_cat) in this series for folder structure and tips.

## Challenge Details

> **Site:** [PicoCTF](https://play.picoctf.org/)  
> **Author:** Juni19  
> **Description:** Flag format: picoCTF{Malwarename} The first letter of the malware name should be capitalized and the rest lowercase. Your friend just got hacked and has been asked to pay some bitcoins to 1Mz7153HMuxXTuR2R1t78mGSdzaAtNbBWX. He doesn’t seem to understand what is going on and asks you for advice. Can you identify what malware he’s being a victim of?  
> **Files:** N/A  
> **Points:** 100  
> *Hint 1: Some crypto-currencies abuse databases exist; check them out!*
> *Hint 2: Maybe Google might help.*

## Investigation

Let's just google "Ransomware 1Mz7153HMuxXTuR2R1t78mGSdzaAtNbBWX" and see what we get. After some browsing we will find that the malware using this bitcoin address was called Petya.

### Flag

> FLAG = picoCTF{Petya}
{: .prompt-tip }