# Cyber FastTrack Spring 2021

### Solver: Will Green ([Ducky](https://github.com/wlg0005))
### Challenge: NM01 
### Category: Networking (Medium)

## Description:
#### [250 pts]: Retrieve output from network endpoint at `cfta-nm01.allyourbases.co` port `8017` and figure out how to get the flag.

## Walkthrough:

Trying to connect to the server with netcat, we're presented the following:

```bash
nc cfta-ne01.allyourbases.co 8017

\x4B\x43\x59\x43\x55\x53
```

That looks like hex. Let's try hex decoding it with [CyberChef](https://gchq.github.io/CyberChef/):

![](NM01%20Writeup.001.png)

Cool, so we get the output `IZBMDR`. If we send this back to the server we get a message that says `Too Slow!`.

Since the server generates a different hex-string each time, we need some way of decoding it and sending it to the server automatically. We can accomplish this by using the python library, [pwntools](https://github.com/Gallopsled/pwntools).

I crafted the following script to solve this:

```python
from pwn import *

# connect to the server
r = remote('cfta-nm01.allyourbases.co',8017)

# receive the hex string and remove \n
prompt = r.recvline().strip()

# remove the \x so we can decode the hex
prompt = "".join([x for x in prompt.decode() if x not in "\\x"])

# decode the hex string
prompt = bytes.fromhex(prompt)

# send the decoded hex to the server
r.sendline(prompt)
r.interactive() # allows us to the see flag
```

Running the script we get the flag from the server:

```bash
python3 get_flag.py

[+] Opening connection to cfta-nm01.allyourbases.co on port 8017: Done
[*] Switching to interactive mode
Correct! - Flag: o[hex]=>i[ascii]=:)[*] Got EOF while reading in interactive
```

### Flag: o[hex]=>i[ascii]=:)
