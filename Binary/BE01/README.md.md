# Cyber FastTrack Spring 2021

### Solver: Will Green ([Ducky](https://github.com/wlg0005))
### Challenge: BE01
### Category: Binary (Easy)

## Description:
#### [100 pts]: Download the file and find a way to get the flag.

## Walkthrough:

Unzipping the provided file, we're presented with a file called `chicken.pdf` which just contains a picture of a chicken:

![](BE01%20Writeup.001.png)

Interesting.. Let's trying using a tool like [foremost](http://manpages.ubuntu.com/manpages/bionic/man8/foremost.8.html) to see if we can find any hidden files:

```bash
foremost chicken.pdf

Processing: chicken.pdf
|foundat=egg.zipPK

*|
```

Nice, it looks like foremost was able to find a ZIP file. Let's try to unzip it:

```bash
unzip 00000000.zip

Archive:  00000000.zip
error [00000000.zip]:  missing 72 bytes in zipfile
  (attempting to process anyway)
error: invalid zip file with overlapped components (possible zip bomb)
```

This had me confused for a little bit, it seemed that foremost didn't properly extract the ZIP file so I was getting errors. I tried just renaming `chicken.pdf` to `chicken.zip` and then unzipping:

```bash
cp chicken.pdf chicken.zip
unzip chicken.zip

Archive:  chicken.zip
 extracting: egg.zip
 ```

 Success! We extracted another file called `egg.zip`. Let's try extracting that one as well:

 ```bash
unzip egg.zip

Archive:  egg.zip
replace chicken.zip? [y]es, [n]o, [A]ll, [N]one, [r]ename:
```

Ah, it looks like it contains another file called `chicken.zip`. You could continue extracting by hand, but without knowing exactly how many zip files are nested within each other, this could be quite the task. So, I crafted a simple script to extract all of them for me:

```python
from zipfile import ZipFile
import os

# while there are still zip files to extract
while True:

    # open chicken.zip
    with ZipFile('chicken.zip', 'r') as zip_ref2:
        zip_ref2.extractall() # extract

    os.remove('chicken.zip') # remove chicken.zip

    # when we extracted chicken.zip, we got egg.zip
    with ZipFile('egg.zip','r') as zip_ref:
        zip_ref.extractall() # extract egg.zip

    os.remove('egg.zip') # remove egg.zip
```

Running the program, we quickly extract all of the files and get a file called `egg.pdf`:

![](BE01%20Writeup.002.png)

Nice, we got the flag! This was an interesting challenge. I'm not sure I'd exactly put it in the "Binary" category. This felt more like a forensics challenge.

### Flag: wh1ch_came_f1rst?

    