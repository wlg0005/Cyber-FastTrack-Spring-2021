# Cyber FastTrack Spring 2021

### Solver: Will Green ([Ducky](https://github.com/wlg0005))
### Challenge: FE03
### Category: Forensics (Easy)

## Description:
#### [100 pts]: Download the file and find a way to get the flag from the docker image.

## Walkthrough:

We're provided a file named `fe03.tar.gz`. This challenge could be solved simply with strings:

```bash
strings fe03.tar | grep -i flag

home/secret/flag.txt
Flag: 8191-SiMpLeFilESysTemForens1Cs
...
zip -P taskdeadlyauditorywoodwork flag.zip flag.txt
rm /home/secret/flag.txt
...
zip -P taskdeadlyauditorywoodwork flag.zip flag.txt
rm /home/secret/flag.txt
home/secret/.wh.flag.txt
home/secret/flag.zip
flag.txtUT
flag.txtUT
 0home/secret/flag.txt
zip -P taskdeadlyauditorywoodwork flag.zip flag.txt
rm /home/secret/flag.txt
...
home/secret/flag.txt
Flag: 8191-SiMpLeFilESysTemForens1Cs
home/secret/flag.txt
Flag: 8191-SiMpLeFilESysTemForens1Cs
```

We could also use a tool like [foremost](http://manpages.ubuntu.com/manpages/bionic/man8/foremost.8.html) to carve out the zip file, and then use the password that we found from strings, `taskdeadlyauditorywoodwork`, to unzip and get `flag.txt`.

### Flag: 8191-SiMpLeFilESysTemForens1Cs
