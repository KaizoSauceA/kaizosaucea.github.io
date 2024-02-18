---
title: PicoCTF 2021 - Magikarp Ground Mission
author: KaizoSauceA
date: 2024-02-17 12:05:00 -0
categories: [CTF,PicoCTF 2021]
tags: [picoctf_general-skills,linux]
---

Refer to the [first entry](../picoctf2021-obedient_cat) in this series for folder structure and tips.

## Challenge Details

> **Site:** [PicoCTF](https://play.picoctf.org/)  
> **Author:** syreal  
> **Description:** Do you know how to move between directories and read files in the shell? Start the container, `ssh` to it, and then `ls` once connected to begin. Login via `ssh` as `ctf-player` with the password, `6d448c9c`Additional details will be available after launching your challenge instance.*  
> **Files:** N/A  
> **Points:** 30  
> * *Hint 1: Finding a cheat sheet for bash would be really helpful!*

## Investigation

Create a folder in the relevant directory, let's create a flag.txt file so we can copy anything we find on the remote server to a local file.

```bash
cd general_skills
mkdir magikarp_ground_mission && cd $_
touch flag.txt
mousepad flag.txt
```

After connecting to our instance and using the password let's do **ls** as instructed and got from there using **cat** and **cd** to navigate. We can find all 3 pieces of the flag and copy them to our flag.txt file.

```bash
ctf-player@pico-chall$ ls
1of3.flag.txt  instructions-to-2of3.txt
ctf-player@pico-chall$ cat 1of3.flag.txt 
picoCTF{xxsh_
ctf-player@pico-chall$ cat instructions-to-2of3.txt 
Next, go to the root of all things, more succinctly `/`
ctf-player@pico-chall$ cd /
ctf-player@pico-chall$ ls
2of3.flag.txt  boot  etc   instructions-to-3of3.txt  lib64  mnt  proc  run   srv  tmp  var
bin            dev   home  lib                       media  opt  root  sbin  sys  usr
ctf-player@pico-chall$ cat 2of3.flag.txt 
0ut_0f_\/\/4t3r_
ctf-player@pico-chall$ cat instructions-to-3of3.txt 
Lastly, ctf-player, go home... more succinctly `~`
ctf-player@pico-chall$ cd ~
ctf-player@pico-chall$ ls
3of3.flag.txt  drop-in
ctf-player@pico-chall$ cat 3of3.flag.txt 
5190b070}
ctf-player@pico-chall$
```

### Flag

> FLAG = picoCTF{xxsh_0ut_0f_\/\/4t3r_5190b070}
{: .prompt-tip }

Mark our folder as solved.

```bash
cd ..
mv magikarp_ground_mission{,_SOLVED}
```