# Cyber FastTrack Spring 2021

### Solver: Will Green ([Ducky](https://github.com/wlg0005))
### Challenge: FE04
### Category: Forensics (Easy)

## Description:
#### [100 pts]: Download the file and filter down to the username according to criteria below.
#### The username you are looking for has `x` as the 3rd character, followed immediately by a number from `2` to `6`, it has a `Z` character in it and the last character is `S`.
#### When you have the username, submit it as the flag.

## Walkthrough:

We're provided a file named `50k-users.txt` which is a file with 50,000 usernames as the name would suggest. So, since we know exactly what we're looking for we can craft a regular expression to solve this for us. 

Using a site like [Regex101](https://regex101.com/), and some googling I came up with the following regular expression:

`^..x[2-6].*Z.*S$`

Brief explanation of the regular expression:

1. `^` = starts with
2. `.` = matches any character (since we don't care about the first 2 characters, but need to check the 3rd)
3. `.` = matches any character ""
4. `x` = matches the character x literally
5. `[2-6]` = matches a single character in the range between 2 and 6, right after the x
6. `.*Z` = match any character up until `Z`
7. `.*S$` = match any character up until `S` and `$` means S needs to be the last character

Once I had a working regular expression, I crafted the following script to go through the file and find me any matches:

```python
import re

# regular expression pattern
p = re.compile('^..x[2-6].*Z.*S$')

# open the 50k-users.txt file
with open('50k-users.txt', 'r') as f:

    # read all lines
    lines = f.readlines()

    # for each line
    for line in lines:

        # find a match
        possible = p.findall(line)

        # if the returned list isn't empty, we found a match
        if possible != []:
            print(possible)
```

Running the script, we get one username as the output which is the flag:

```bash
python find_username.py
['YXx52hsi3ZQ5b9rS']
```

Pretty cool challenge that required me to learn a few things and brush up on my regular expression skills.

### Flag: YXx52hsi3ZQ5b9rS 
