# Cyber FastTrack Spring 2021

### Solver: Will Green ([Ducky](https://github.com/wlg0005))
### Challenge: BM02
### Category: Binary (Medium)

## Description:
#### [250 pts]: Download the file and find a way to get the flag.

## Walkthrough:

Running the provided binary, we're presented with the following:

```bash
./program

I'm not going to make it that easy for you.
```

So, let's open the program in a disassembler such as [Binary Ninja](https://binary.ninja/). The main function looks like what we would expect based off running the program:

```cpp
int32_t main(int32_t arg1, char** arg2, char** arg3)

000007eb  int32_t var_c = 1
0000080e  puts(str: "I'm not going to make it that ea…")
00000819  return 0
```

But if we take a look at the other functions, we find an interesting one called `printFlag`! Let's see what that one does:

```cpp
int64_t printFlag(int32_t arg1)

000006b5  void* fsbase
000006b5  int64_t rax = *(fsbase + 0x28)
000006c4  if (arg1 == 0x539)
000006d1      char var_28 = 0x15
000006d5      char var_27_1 = 0x70
000006d9      char var_26_1 = 0xe5
000006dd      char var_25_1 = 0x64
000006e1      char var_24_1 = 0x7a
000006e5      char var_23_1 = 0xd4
000006e9      char var_22_1 = 0x6d
000006ed      char var_21_1 = 0x75
000006f1      char var_20_1 = 0xeb
000006f5      char var_1f_1 = 0xf4
000006f9      char var_1e_1 = 0x6a
000006fd      char var_1d_1 = 0xd1
00000701      char var_1c_1 = 0xfa
00000705      char var_1b_1 = 0xd1
00000709      char var_1a_1 = 0xf9
0000070d      char var_19_1 = 0xe8
00000711      char var_18_1 = 0x9d
00000715      char var_17_1 = 0x7c
00000719      char var_16_1 = 0x41
000007ba      for (int32_t var_2c_1 = 0; var_2c_1 u<= 0x12; var_2c_1 = var_2c_1 + 1)
0000074a          char var_2d_7 = not.b(neg.b(((not.b(*(&var_28 + zx.q(var_2c_1))) + var_2c_1.b) ^ 0x48) - var_2c_1.b))
00000773          uint8_t var_2d_12 = (((((zx.d(var_2d_7) << 3).b | var_2d_7 u>> 5) - var_2c_1.b) ^ 0x5d) - 0x23) ^ var_2c_1.b
000007ae          *(&var_28 + zx.q(var_2c_1)) = ((zx.d(((var_2d_12 + var_2d_12) | var_2d_12 u>> 7) - 0x41) << 5).b | (((var_2d_12 + var_2d_12) | var_2d_12 u>> 7) - 0x41) u>> 3) ^ 0x65
000007c7      puts(str: &var_28)
000007d1  int64_t rax_22 = rax ^ *(fsbase + 0x28)
000007e2  if (rax_22 == 0)
000007e2      return rax_22
000007dc  __stack_chk_fail()
000007dc  noreturn
```

So it prints the flag as you would expect, but we need to pass `0x539` or `1337` as an argument: `if (arg1 == 0x539)`.

We can accomplish this with gdb. We can open the program in gdb like so:

`gdb program`

And then we can set a breakpoint at main, so that we can execute the program but it won't close at the main function:

`break main`

And finally, we can execute the program:

`r`

Cool, so we should now be able to access the `printFlag()` function by first casting it to the correct type (since printFlag doesn't actually return anything, we can just cast to void) and then supplying `1337` as the argument:

```bash
(gdb) p (void) printFlag(1337)
Flag: patchItFixIt
$1 = void
```

Nice!

### Flag: patchItFixIt