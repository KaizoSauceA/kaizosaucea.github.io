---
title: PicoMini 2022 - RunMe.py
author: KaizoSauceA
date: 2024-02-12 09:00:00 -0
categories: [CTF,PicoMini 2022]
tags: [picoctf_general-skills,linux,python]
---

Refer to the [first entry](../picoctf2021-obedient_cat) in this series for folder structure and tips.

## Challenge Details

> **Site:** [PicoCTF](https://play.picoctf.org/)  
> **Author:** Sujeet Kumar  
> **Description:** Run the runme.py script to get the flag. Download the script with your browser or with wget in the webshell.   
> **Files:** [runme.py](https://artifacts.picoctf.net/c/34/runme.py)  
> **Points:** 100  
> * *Hint 1: If you have Python on your computer, you can download the script normally and run it. Otherwise, use the wget command in the webshell.*  
> * *Hint 2: To use wget in the webshell, first right click on the download link and select 'Copy Link' or 'Copy Link Address'*  
> * *Hint 3: Type everything after the dollar sign in the webshell: $ wget , then paste the link after the space after wget and press enter. This will download the script for you in the webshell so you can run it!*  
> * *Hint 4: Finally, to run the script, type everything after the dollar sign and then press enter: $ python3 runme.py You should have the flag now!*  

## Investigation

Create a folder in the relevant directory, download the py file.

```bash
cd general_skills
mkdir fixme1 && cd $_
wget https://artifacts.picoctf.net/c/34/runme.py
```

Execute the file. Flag obtained. This is a very simple challenge to learn some basic commands.

```bash
python runme.py 
picoCTF{run_s4n1ty_run}
```

### Flag

> FLAG = picoCTF{run_s4n1ty_run}
{: .prompt-tip }

Mark our folder as solved.

```bash
cd ..
mv fixme1{,_SOLVED}
```