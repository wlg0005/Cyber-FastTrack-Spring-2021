# Cyber FastTrack Spring 2021

### Solver: Will Green ([Ducky](https://github.com/wlg0005))
### Challenge: WE02 
### Category: Web (Easy)

## Description:
#### [100 pts]: View the page at [https://cfta-we02.allyourbases.co](https://cfta-we02.allyourbases.co) and try to get the flag.

## Walkthrough:

Navigating to the provided URL, we're presented with a simple boiler plate site:

![](WE02%20Writeup.001.png)

There's not much interesting on the site besides this block under `/News` that mentions a secret link:

![](WE02%20Writeup.002.png)

We could use a tool such as [Dirsearch](https://github.com/maurosoria/dirsearch) to find the secret link, but since this was an easy challenge I had a hunch that it was probably `/robots.txt`. So let's take a look at `/robots.txt`:

```
User-agent: \*

Allow: /
Disallow: /4ext6b6.html
```

Ah, it looks we've found another page. Let's navigate there:

![](WE02%20Writeup.003.png)

Nice and easy challenge. Always a good idea to check out `/robots.txt` during CTF competitions.

### Flag: Shhh_robot_you_said_too_much!


