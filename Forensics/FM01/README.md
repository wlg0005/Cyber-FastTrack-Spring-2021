# Cyber FastTrack Spring 2021

### Solver: Will Green ([Ducky](https://github.com/wlg0005))
### Challenge: FM01
### Category: Forensics (Medium)

## Description:
#### [250 pts]: Download the file and find a way to get the flag.

## Walkthrough:

Opening the provided image file we're presented with the following:

![](FM01%20Writeup.001.png)

Interesting.. It just seems like a bunch of weird noise. Using exiftool, we can get the flag:

```bash
exiftool fm01.jpg

ExifTool Version Number         : 12.10
File Name                       : fm01.jpg
...
Text Layer Name                 : flag: tr4il3r_p4rk
Text Layer Text                 : flag: tr4il3r_p4rk
...
```

I think this solution was unintended so check out this [writeup](https://github.com/Alic3C/Cyber-FastTrack-Spring-2021/tree/main/Forensics/FM01) for probably the intended.

### Flag: tr4il3r_p4rk