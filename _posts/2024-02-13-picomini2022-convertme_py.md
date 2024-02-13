---
title: PicoMini 2022 - ConvertMe.py
author: KaizoSauceA
date: 2024-02-13 16:00:00 -0
categories: [CTF,PicoMini 2022]
tags: [picoctf_general-skills,linux,python]
---

Refer to the [first entry](../picoctf2021-obedient_cat) in this series for folder structure and tips.

## Challenge Details

> **Site:** [PicoCTF](https://play.picoctf.org/)  
> **Author:** LT 'syreal' Jones  
> **Description:** Run the Python script and convert the given number from decimal to binary to get the flag.  
> **Files:** [convertme.py](https://artifacts.picoctf.net/c/22/convertme.py)  
> **Points:** 100  
> * *Hint 1: Look up a decimal to binary number conversion app on the web or use your computer's calculator!*  
> * *Hint 2: The str_xor function does not need to be reverse engineered for this challenge.*  
> * *Hint 3: If you have Python on your computer, you can download the script normally and run it. Otherwise, use the wget command in the webshell.*  
> * *Hint 4: To use wget in the webshell, first right click on the download link and select 'Copy Link' or 'Copy Link Address'*  
> * *Hint 5: Type everything after the dollar sign in the webshell: $ wget , then paste the link after the space after wget and press enter. This will download the script for you in the webshell so you can run it!*  
> * *Hint 6: Finally, to run the script, type everything after the dollar sign and then press enter: $ python3 convertme.py*

## Investigation

Create a folder in the relevant directory, download all the files.

```bash
cd general_skills
mkdir convertme_py && cd $_
wget https://artifacts.picoctf.net/c/22/convertme.py
```

Let's run the py file. We get a prompt for a decimal to binary conversion. Using the calculator at [RapidTables](https://www.rapidtables.com/convert/number/decimal-to-binary.html?x=12) we get the answer. Enter it as the answer and we get the flag.

```bash
python convertme.py 
If 12 is in decimal base, what is it in binary base?
Answer: 1100              
That is correct! Here's your flag: picoCTF{4ll_y0ur_b4535_762f748e}
```

### The Python File

If we open the file itself we can see that there is a random number every time as the prompt. Let's create a tinker.py copy of the file and see if we can just break the file and get the flag without any effort. The below alterations in the new file below the existing function will print out the decoded key and bypass the prompt and checks to give us the flag two different ways without needing to do any binary calculations.

```python
# print out the decoded flag straight away
print(str_xor(flag_enc, 'enkidu'))

# change to hard coded values
num = "ABC"
ans = "ABC"

try:
  if ans == num:
    flag = str_xor(flag_enc, 'enkidu')
    print('That is correct! Here\'s your flag: ' + flag)
  else:
    print(str(num) + ' and ' + str(num) + ' are not equal.')
except ValueError:
  print('That isn\'t a binary number. Binary numbers contain only 1\'s and 0\'s')
```

### Flag

> FLAG = picoCTF{4ll_y0ur_b4535_762f748e}
{: .prompt-tip }

Mark our folder as solved.

```bash
cd ..
mv convertme_py{,_SOLVED}
```