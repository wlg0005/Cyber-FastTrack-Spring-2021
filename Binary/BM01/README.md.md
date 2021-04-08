# Cyber FastTrack Spring 2021

### Solver: Will Green ([Ducky](https://github.com/wlg0005))
### Challenge: BM01
### Category: Binary (Medium)

## Description:
#### [250 pts]: Download the file and find a way to get the flag.

## Walkthrough:

Running the provided binary, we're presented with the following:

```bash
./program

Какой пароль？
>
```

Interesting.. I do not speak Russian so I did a quick google translate to find out that the program is asking for a password. Let's try opening the program in a disassembler like [Binary Ninja](https://binary.ninja/):

```cpp
int32_t main(int32_t arg1, char** arg2, char** arg3)

000007d2  void* fsbase
000007d2  int64_t rax = *(fsbase + 0x28)
000007f3  puts(str: data_9e0)
00000804  printf(format: data_a04)
0000081c  void var_58
0000081c  fgets(buf: &var_58, n: 0x3c, fp: *stdin)
00000834  if (strcmp(data_9c8, &var_58) != 0)
00000911      puts(str: data_a37)
0000083c  else
0000083c      char var_67 = 0xe4
00000840      char var_66_1 = 0x64
00000844      char var_65_1 = 0xa6
00000848      char var_64_1 = 0x90
0000084c      char var_63_1 = 0x7c
00000850      char var_62_1 = 0xa6
00000854      char var_61_1 = 0x75
00000858      char var_60_1 = 0xb8
0000085c      char var_5f_1 = 0xa4
00000860      char var_5e_1 = 0xd
00000864      char var_5d_1 = 0xc
00000868      char var_5c_1 = 0x7f
0000086c      char var_5b_1 = 0x7e
00000870      char var_5a_1 = 0xf3
00000874      char var_59_1 = 1
000008ee      for (int32_t var_74_1 = 0; var_74_1 u<= 0xe; var_74_1 = var_74_1 + 1)
000008ad          char var_75_10 = not.b(not.b(not.b(neg.b(((*(&var_67 + zx.q(var_74_1)) ^ 0xa5) - var_74_1.b) ^ var_74_1.b)) ^ 0x8d) - 0xb)
000008e2          *(&var_67 + zx.q(var_74_1)) = ((((((zx.d(var_75_10) << 5).b | var_75_10 u>> 3) + 0x37) ^ 0xe5) - 7) ^ var_74_1.b) - 0x39
00000903      printf(format: data_a08, &var_67)
00000930  if ((rax ^ *(fsbase + 0x28)) == 0)
00000930      return 0
0000092a  __stack_chk_fail()
0000092a  noreturn
```

So, it looks like the program takes our input and then does a simple strcmp() on it with some data:

`if (strcmp(data_9c8, &var_58) != 0)`

strcmp() returns 0 when the two input strings are equal to each other, so when this if statement evaluates to true, the following error message is displayed: `неверный.` Google translating, again, reveals that this means "incorrect".

So, we need to figure out what our input is being compared to. We can click the `data_9c8` in Binary Ninja to take us to where that information is stored:

```cpp
000009c8  data_9c8:
000009c8                          d0 bc d0 be d0 bb d0 be d1 82 d0 be d0 ba 31 32 33 0a 00 00 00 00 00 00          ..............123.......
```

Cool, so this is the data that is being compared to our input, but Binary Ninja doesn't seem to be able to display some of the characters because they are in Russian. We can use [CyberChef](https://gchq.github.io/CyberChef/) to decode the hex:

![](BM01%20Writeup.001.png)

Cool, that looks like the password! Let's trying sending that to the program with 123 added to the end:

```bash
./program

Какой пароль？
> молоток123
верный!

флаг: wh1te%BluE$R3d
```

Nice, we got the flag! Out of curiosity, I google translated the password and it seems to translate to `hammer123`

### Flag: wh1te%BluE$R3d