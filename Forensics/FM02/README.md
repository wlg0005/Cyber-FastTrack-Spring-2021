# Cyber FastTrack Spring 2021

### Solver: Will Green ([Ducky](https://github.com/wlg0005))
### Challenge: FM01
### Category: Forensics (Medium)

## Description:
#### [250 pts]: Download the file and find a way to get the flag.

## Walkthrough:

We're provided a pcapng file named `IRC-cap-vpn.pcapng`. Since IRC was in the name of the file, the first thing I did was sort by protocol in Wireshark and scrolled till I found some IRC packets (tcp.stream eq 57). Following the TCP stream we get the following output:

```
  

ISON RandumbHero1

:barjavel.freenode.net 303 RiotCard851 :RandumbHero1

ISON RandumbHero1

:barjavel.freenode.net 303 RiotCard851 :RandumbHero1

ISON RandumbHero1

:barjavel.freenode.net 303 RiotCard851 :RandumbHero1

PRIVMSG RandumbHero1 :Hey man, How's it going?

:RandumbHero1!~User@82.102.19.124 PRIVMSG RiotCard851 :All good, how are you?

ISON RandumbHero1

:barjavel.freenode.net 303 RiotCard851 :RandumbHero1

PRIVMSG RandumbHero1 :yeah Doing good, been working on something recently. Wanna check it out?

:RandumbHero1!~User@82.102.19.124 PRIVMSG RiotCard851 :Sure, What is it?

ISON RandumbHero1

:barjavel.freenode.net 303 RiotCard851 :RandumbHero1

PRIVMSG RandumbHero1 :See if you can work it out first. I've hidden the flag in it. ;)

PRIVMSG RandumbHero1 :.DCC SEND "Flag.7z" 3232247681 35289 3466.

ISON RandumbHero1

:barjavel.freenode.net 303 RiotCard851 :RandumbHero1

PRIVMSG RandumbHero1 :here you go!

PRIVMSG RandumbHero1 :Password on it, using the trick as usual.

ISON RandumbHero1

:barjavel.freenode.net 303 RiotCard851 :RandumbHero1

PRIVMSG RandumbHero1 :TWFyaW9SdWxlejE5ODU=

:RandumbHero1!~User@82.102.19.124 PRIVMSG RiotCard851 :Awesome, I'll go check it out now.
```

So it looks like there's two users, RandumbHero1 and RiotCard851, who are sharing a 7zip file between each other with a password. The very last message contains `TWFyaW9SdWxlejE5ODU=` which looks like Base64, so let's trying decoding that:

```bash
echo TWFyaW9SdWxlejE5ODU= | base64 -d

MarioRulez1985
```

Nice, that looks like a password. Now we just need to find the packets containing the 7zip file. I like to refer to this [page](https://www.7-zip.org/recover.html) whenever I have questions about the 7zip file format. Looking at that page we can figure out that the the 7zip file signature in hex is: `37 7A BC AF 27 1C`

We can search by hex value in Wireshark which takes us right to the packet containing the 7zip file (#2863, tcp.stream eq 79). Following the TCP stream, we can select "Show and save data as: Raw" to save the raw bytes, and save the file with a .7z extension.

Now it's as simple as unzipping the file with the password we found earlier with 7zip:

![](FM02%20Writeup.001.png)

Doing so, we get a file named Flag.nes. We can just run strings and grep for flag to find the flag:

```bash
strings Flag.nes | grep -i flag

You have found the flag!
but you found the flag!
Flag: NESted_in_a_PCAP
```

Nice and easy pcap challenge!

### Flag: NESted_in_a_PCAP

