# Cyber FastTrack Spring 2021

### Solver: Will Green ([Ducky](https://github.com/wlg0005))
### Challenge: NE01
### Category: Networking (Easy)

## Description:
#### [250 pts]: There is a TCP network service running on `cfta-ne01.allyourbases.co`. Find it to get the flag after you connect.
#### Note: The target has many open ports - only one is the correct one. The correct port will identify itself with `ID: ne01` after you connect.

## Walkthrough:

So we need to find which port this server is using, we can do a simple Nmap scan to figure out which ports are open:

```bash
nmap -Pn cfta-ne01.allyourbases.co

Starting Nmap 7.70 ( https://nmap.org ) at 2021-04-08 11:29
Nmap scan report for cfta-ne01.allyourbases.co (52.210.101.4
Host is up (0.13s latency).
Other addresses for cfta-ne01.allyourbases.co (not scanned):
rDNS record for 52.210.101.44: ec2-52-210-101-44.eu-west-1.c
Not shown: 999 filtered ports
PORT     STATE SERVICE
1061/tcp open  kiosk

Nmap done: 1 IP address (1 host up) scanned in 19.65 seconds
```

Cool, Nmap was only able to find one port so this makes our lives a lot easire. We can use netcat to connect to the server with port `1061`:

```bash
nc cfta-ne01.allyourbases.co 1061

ID: ne01
Flag: Nmap_0f_the_W0rld!
```

Nice, there's the flag!

### Flag: Nmap_0f_the_W0rld!