---
title: PicoCTF 2021 - Nice Netcat
author: KaizoSauceA
date: 2024-02-10 11:00:00 -0
categories: [CTF,PicoCTF 2021]
tags: [picoctf_general-skills,linux,python]
---

Refer to the [first entry](../picoctf2021-obedient_cat) in this series for folder structure and tips.

## Challenge Details

> **Site:** [PicoCTF](https://play.picoctf.org/)  
> **Author:** syreal  
> **Description:** There is a nice program that you can talk to by using this command in a shell: $ nc mercury.picoctf.net 43239, but it doesn't speak English...  
> **Files:** N/A 
> **Points:** 15  
> * *Hint 1: You can practice using netcat with this picoGym problem: [what's a netcat?](https://play.picoctf.org/practice/challenge/34)*
> * *Hint 2: You can practice reading and writing ASCII with this picoGym problem: [Let's Warm Up](https://play.picoctf.org/practice/challenge/22)*

## Investigation

Create a folder in the relevant directory and connect to the site. The program spits out a series of numbers. Let's save those into a file called **data.txt**.

```bash
cd general_skills
mkdir nice_netcat && cd $_
nc mercury.picoctf.net 43239 > data.txt
```

Hint 2 mentions ASCII which likely means that these numbers can be converted to their alphanumeric counterparts to generate the flag string. Lets create a new python file **decode.py**.

We need to read the **data.txt** file, capturing all of the numbers. After that we strip away all of the spaces and newline characters and convert to integers. Finally we need to convert all of the integers to ASCII and create the flag string. The code below accomplishes it, but there are many ways to go about it.

```python
# empty list to append converted characters to
data = []

# open the data file and read all lines into a raw list
with open("data.txt", "r") as file:
    raw = file.readlines()

    # iterate over raw, strip newlines and convert to integer and append to data
    for item in raw:
        data.append(int(item.strip()))

# use string join method and list comprehension to create the flag string
output = "".join(chr(n).strip() for n in data)

print(output)
```

Running the python file generates the flag

```bash
python decode.py
picoCTF{g00d_k1tty!_n1c3_k1tty!_7c0821f5}
```

### Flag

> FLAG = picoCTF{g00d_k1tty!_n1c3_k1tty!_7c0821f5}
{: .prompt-tip }

Mark our folder as solved.

```bash
cd ..
mv nice_netcat{,_SOLVED}
```