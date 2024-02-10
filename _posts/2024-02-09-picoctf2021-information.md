---
title: WIP PicoCTF 2021 - Information
author: KaizoSauceA
date: 2024-02-10 10:00:00 -0
categories: [CTF,PicoCTF 2021]
tags: [picoctf_forensics,linux]
---

Refer to the [first entry](../picoctf2021-obedient_cat) in this series for folder structure and tips.

## Challenge Details

> **Site:** [PicoCTF](https://play.picoctf.org/)  
> **Author:** susie  
> **Description:** Files can always be changed in a secret way. Can you find the flag? cat.jpg  
> **Files:** [cat.jpg](https://mercury.picoctf.net/static/b4d62f6e431dc8e563309ea8c33a06b3/cat.jpg)  
> **Points:** 10  
> * *Hint 1: Look at the details of the file.*
> * *Hint 2: Make sure to submit the flag as picoCTF{XXXXX}.*

## Investigation

Create a folder in the relevant directory and download the file.

```bash
cd forensics
mkdir information && cd $_
wget https://mercury.picoctf.net/static/b4d62f6e431dc8e563309ea8c33a06b3/cat.jpg
```

Opening the image reveals a standard image of a cat
![cat.jpg](https://mercury.picoctf.net/static/b4d62f6e431dc8e563309ea8c33a06b3/cat.jpg)

Let's investigate the details of the file

```bash
exiftool cat.jpg

xifTool Version Number         : 12.70
File Name                       : cat.jpg
Directory                       : .
File Size                       : 878 kB
File Modification Date/Time     : 2021:03:15 14:24:46-04:00
File Access Date/Time           : 2024:02:05 01:42:33-05:00
File Inode Change Date/Time     : 2024:02:05 01:42:26-05:00
File Permissions                : -rw-r--r--
File Type                       : JPEG
File Type Extension             : jpg
MIME Type                       : image/jpeg
JFIF Version                    : 1.02
Resolution Unit                 : None
X Resolution                    : 1
Y Resolution                    : 1
Current IPTC Digest             : 7a78f3d9cfb1ce42ab5a3aa30573d617
Copyright Notice                : PicoCTF
Application Record Version      : 4
XMP Toolkit                     : Image::ExifTool 10.80
License                         : cGljb0NURnt0aGVfbTN0YWRhdGFfMXNfbW9kaWZpZWR9
Rights                          : PicoCTF
Image Width                     : 2560
Image Height                    : 1598
Encoding Process                : Baseline DCT, Huffman coding
Bits Per Sample                 : 8
Color Components                : 3
Y Cb Cr Sub Sampling            : YCbCr4:2:0 (2 2)
Image Size                      : 2560x1598
Megapixels                      : 4.1
```

### Online Decoder

The **Current IPTC Digest** and **License** fields look like encrypted strings. Maybe something like base64. Let's look further. Pasting them both into the decoder at [base64decode](https://www.base64decode.org/) reveals the license string is the flag.

### Local Terminal

We can achieve the same result using the terminal on our local machine by echoing the encoded string and piping it into the **base64** program with the **-d** flag.

```bash
echo cGljb0NURnt0aGVfbTN0YWRhdGFfMXNfbW9kaWZpZWR9 | base64 -d
picoCTF{the_m3tadata_1s_modified}
```

### Flag

> FLAG = picoCTF{the_m3tadata_1s_modified}
{: .prompt-tip }

Mark our folder as solved.

```bash
cd ..
mv information{,_SOLVED}
```