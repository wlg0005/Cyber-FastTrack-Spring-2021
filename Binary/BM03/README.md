# Cyber FastTrack Spring 2021

### Solver: Will Green ([Ducky](https://github.com/wlg0005))
### Challenge: BM03
### Category: Binary (Medium)

## Description:
#### [250 pts]: Download the file and find a way to get the flag.

## Walkthrough:

Running the provided binary, we're presented with the following:

```bash
./flag

 Flag:
       __       __                          _                      ____ __
  ____/ /___   / /_   __  __ ____ _ ____ _ (_)____   ____ _       / __// /_ _      __
 Error displaying rest of flag
```

So it seems that we need to find some way to display the rest of the flag. Let's throw the program into a disassembler like [Binary Ninja](https://binary.ninja/). The main function looks like this:

```cpp
int32_t main(int32_t arg1, char** arg2, char** arg3)

000008df  fflush(fp: *stdout)
000008eb  puts(str: "\n\x1b[36m Flag:\x1b[0m")
000008f0  int32_t var_10 = 2
000008f7  int32_t var_c = 0x55
00000908  output(2, 0x55)
00000913  return 0
```

So it seems that the `output` function is what prints out the flag, and it is initially passed two arguments, `2` and `0x55`. Let's take a look at the output function:

```cpp
int64_t output(int32_t arg1, int32_t arg2)

000007a1  void* fsbase
000007a1  int64_t rax = *(fsbase + 0x28)
000007be  int64_t rcx = 0xff
000007c3  void var_818
000007c3  void* rdi = &var_818
000007c6  void* rsi = data_a00
000007c9  for (; rcx != 0; rcx = rcx - 1)
000007c9      *rdi = *rsi
000007c9      rdi = rdi + 8
000007c9      rsi = rsi + 8
000007cc  char var_1b = 0x20
000007d0  char var_1a = 0x5f
000007d4  char var_19 = 0x2f
000007d8  char var_18 = 0x5c
000007dc  char var_17 = 0x28
000007e0  char var_16 = 0x29
000007e4  char var_15 = 0x60
000007e8  char var_14 = 0x2c
000007ec  char var_13 = 0x7c
000007f0  char var_12 = 0x2e
000007f4  char var_11 = 0
0000089b  for (int32_t var_820 = 0; var_820 s< arg1; var_820 = var_820 + 1)
0000087c      for (int32_t var_81c_1 = 0; var_81c_1 s< arg2; var_81c_1 = var_81c_1 + 1)
00000822          int64_t rdx_1 = sx.q(var_820)
0000082c          int64_t rax_5 = (rdx_1 << 2) + rdx_1
0000083c          int32_t rcx_2 = *(&var_818 + ((rax_5 + (rax_5 << 4) + sx.q(var_81c_1)) << 2))
0000084a          int32_t temp0_1
0000084a          int32_t temp1_1
0000084a          temp0_1:temp1_1 = muls.dp.d(rcx_2, 0x51eb851f)
00000864          putchar(c: sx.d(*(&var_1b + sx.q((temp0_1 s>> 5) - (rcx_2 s>> 0x1f)))))
00000883      putchar(c: 0xa)
000008a1  if (arg1 s<= 5)
000008b1      puts(str: "\x1b[31m Error displaying rest o…")
000008bb  int64_t rax_18 = rax ^ *(fsbase + 0x28)
000008cc  if (rax_18 == 0)
000008cc      return rax_18
000008c6  __stack_chk_fail()
000008c6  noreturn
```

This looks like the rest of functions that have printed the flag in the other challenges, but there is one important condition towards the bottom:

```cpp
000008a1  if (arg1 s<= 5)
000008b1      puts(str: "\x1b[31m Error displaying rest o…")
```

So it seems that since we are only passing in `2` as the first argument, we are only able to see the first 2 rows of the flag, and in this case an error message is also shown if the first argument is less than 6.

We can use gdb to run the function manually and supply the proper argument value:

```bash
gdb flag
...
(gdb) break main
...
(gdb) r
...
(gdb) p output(6,0x55)
       __       __                          _                      ____ __
  ____/ /___   / /_   __  __ ____ _ ____ _ (_)____   ____ _       / __// /_ _      __
 / __  // _ \ / __ \ / / / // __ `// __ `// // __ \ / __ `/      / /_ / __/| | /| / /
/ /_/ //  __// /_/ // /_/ // /_/ // /_/ // // / / // /_/ /      / __// /_  | |/ |/ /
\__,_/ \___//_.___/ \__,_/ \__, / \__, //_//_/ /_/ \__, /______/_/   \__/  |__/|__/
                          /____/ /____/           /____//_____/
$1 = void
```

Nice, that's the flag!

### Flag: debugging_ftw