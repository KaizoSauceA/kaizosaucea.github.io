---
title: WIP PicoCTF 2021 - Wave a Flag
author: KaizoSauceA
date: 2024-02-10 09:00:00 -0
categories: [CTF,PicoCTF 2021]
tags: [picoctf_general-skills,linux]
---

Refer to the [first entry](../picoctf2021-obedient_cat) in this series for folder structure and tips.

## Challenge Details

> **Site:** [PicoCTF](https://play.picoctf.org/)  
> **Author:** syreal  
> **Description:** Can you invoke help flags for a tool or binary? This program has extraordinarily helpful information...  
> **Files:** [warm](https://mercury.picoctf.net/static/a14be2648c73e3cda5fc8490a2f476af/warm)  
> **Points:** 10  
> * *Hint 1: This program will only work in the webshell or another Linux computer.*  
> * *Hint 2: To get the file accessible in your shell, enter the following in the Terminal prompt: $ wget https://mercury.picoctf.net/static/a14be2648c73e3cda5fc8490a2f476af/warm*  
> * *Hint 3: Run this program by entering the following in the Terminal prompt: $ ./warm, but you'll first have to make it executable with $ chmod +x warm*  
> * *Hint 4: -h and --help are the most common arguments to give to programs to get more information from them!*
> * *Hint 5: Not every program implements help features like -h and --help.*

## Investigation

Create a folder in the relevant directory and download the file.

```bash
cd general_skills
mkdir wave_a_flag && cd $_
wget https://mercury.picoctf.net/static/a14be2648c73e3cda5fc8490a2f476af/warm
```

If we try to run the file we get a permission error, so let's make the file executable.

```bash
./warm
zsh: permission denied: ./warm
# make file executable
chmod +x warm
```

Now we are able to run the file and we are prompted to pass a **-h** argument. If we do that we get the output of the flag. A simple lesson.

```bash
./warm
Hello user! Pass me a -h to learn what I can do!
# rerun with the -h argument
./warm -h
Oh, help? I actually don't do much, but I do have this flag here: picoCTF{b1scu1ts_4nd_gr4vy_755f3544}
```

### Flag

> FLAG = picoCTF{b1scu1ts_4nd_gr4vy_755f3544}
{: .prompt-tip }

Mark our folder as solved.

```bash
cd ..
mv wave_a_flag{,_SOLVED}
```