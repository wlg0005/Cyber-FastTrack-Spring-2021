# Cyber FastTrack Spring 2021

### Solver: Will Green ([Ducky](https://github.com/wlg0005))
### Challenge: CM01
### Category: Crypto (Medium)

## Description:
#### [250 pts]: Download the file and find a way to get the flag.

## Walkthrough:

Extracting the given zip file we're presented with two images:

![](CM01%20Writeup.001.png)

Hm, so it looks like we're given `frame.png` which is a QR code and `code.png` which looks very similar to a QR code but is obviously malformed. 

If you scan `frame.png`, you get the following text: `Hey, I've put the flag into the other file using the same trick we always use. You know what to do. :)`

So, we know that flag is within `code.png`. One of the first things I noticed was that the border of `code.png` is black while the border of `frame.png`. This made me think of the bitwise operation XOR which evaluates to true when the number of true inputs is odd. So if we imagine that the color black = 0 and the color white = 1, then the output would be white (1). 

With that understanding, let's trying to XOR these images together! I used a tool called [StegSolve](https://github.com/zardus/ctf-tools/blob/master/stegsolve/install) to perform this operation for me. Opening up `code.png` in StegSolve, and then choosing `Analyse -> Image Combiner` shows the XOR of the two images:

![](CM01%20Writeup.002.png)

That looks like a new QR code! Let's save it as `flag.png` and then scan it using [zbarimg](http://manpages.ubuntu.com/manpages/bionic/man1/zbarimg.1.html):

```bash
zbarimg flag.png

Connection Error (Failed to connect to socket /run/dbus/system_bus_socket: No such file or directory)
Connection Null
QR-Code:FLAG: A_Code_For_A_Code
scanned 1 barcode symbols from 1 images in 0.02 seconds
```

Nice, we got the flag!

### Flag: A_Code_For_A_Code