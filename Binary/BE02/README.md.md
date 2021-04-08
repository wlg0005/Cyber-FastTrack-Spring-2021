# Cyber FastTrack Spring 2021

### Solver: Will Green ([Ducky](https://github.com/wlg0005))
### Challenge: BE02
### Category: Binary (Easy)

## Description:
#### [100 pts]: Download the file and find a way to get the flag.

## Walkthrough:

Running the provided binary, we're presented with the following:

```bash
./rot13

===================================
ROT IN sHELL :: ROT13's your input!
===================================
>
```

So, the program just returns the ROT13 cipher for your input text. During the competition, I had a hunch that this would probably be a simple buffer overflow since it was an easy challenge. So I tried spamming my keyboard with `A` characters:

```bash
./rot13

===================================
ROT IN sHELL :: ROT13's your input!
===================================
> AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA

Segmentation fault.
*** stack smashing detected ***: <unknown> terminated
Aborted (core dumped)

Flag: luckyNumber13
```

Sure enough, it was that simple! 

You could also open the program in a disassembler such as [Binary Ninja](https://binary.ninja/) and see that the program actually just checks for whether or not the input is less than or equal to 32 characters (hex: 0x20):

```cpp
0000090c  fgets(buf: &var_88, n: 0x64, fp: *stdin)
00000921  *(&var_88 + strlen(&var_88) - 1) = 0
00000932  if (strlen(&var_88) u<= 0x20)
```

Otherwise, if the input is more than 32 characters it will print the flag in a rather ugly way:

```cpp
00000950  else
00000950      int64_t var_128 = 0x61746e656d676553
00000957      int64_t var_120_1 = 0x756166206e6f6974
0000095e      int32_t var_118_1 = 0x2e746c
0000097c      int64_t var_c8 = 0x63617473202a2a2a
00000983      int64_t var_c0_1 = 0x696873616d73206b
0000099e      int64_t var_b8_1 = 0x636574656420676e
000009a5      int64_t var_b0_1 = 0x3a2a2a2a20646574
000009c0      int64_t var_a8_1 = 0x776f6e6b6e753c20
000009c7      int64_t var_a0_1 = 0x696d726574203e6e
000009ce      int32_t var_98_1 = 0x6574616e
000009d8      int16_t var_94_1 = 0x64
000009eb      uint64_t rdx_1 = 0x75642065726f6328
000009f5      int64_t var_e8 = 0x20646574726f6241
000009fc      int64_t var_e0_1 = 0x75642065726f6328
00000a03      int32_t var_d8_1 = 0x6465706d
00000a0d      int16_t var_d4_1 = 0x29
00000a16      char var_108 = 0x77
00000a1d      char var_107_1 = 0xf3
00000a24      char var_106_1 = 0xdb
00000a2b      char var_105_1 = 0xff
00000a32      char var_104_1 = 0x38
00000a39      char var_103_1 = 0xd2
00000a40      char var_102_1 = 0xef
00000a47      char var_101_1 = 0xf
00000a4e      char var_100_1 = 0xeb
00000a55      char var_ff_1 = 0xc7
00000a5c      char var_fe_1 = 0x1b
00000a63      char var_fd_1 = 0xb3
00000a6a      char var_fc_1 = 0x33
00000a71      char var_fb_1 = 0xd7
00000a78      char var_fa_1 = 0xf7
00000a7f      char var_f9_1 = 0xdf
00000a86      char var_f8_1 = 0x47
00000a8d      char var_f7_1 = 0x5e
00000a94      char var_f6_1 = 0x30
00000a9b      char var_f5_1 = 0xf5
00000b70      for (int32_t var_134_1 = 0; var_134_1 u<= 0x13; var_134_1 = var_134_1 + 1)
00000b0f          uint8_t var_135_12 = neg.b(not.b(neg.b(not.b(((not.b(neg.b(*(&var_108 + zx.q(var_134_1)))) - 0x4b) ^ var_134_1.b) - 0x1b) - var_134_1.b))) ^ 0x13
00000b54          rdx_1 = zx.q(((((zx.d(var_135_12) << 6).b | var_135_12 u>> 2) + 0x38) ^ var_134_1.b) - 0x32)
00000b5b          *(&var_108 + zx.q(var_134_1)) = rdx_1.b
00000b8c      printf(format: "\n\x1b[31m%s\n", &var_128, rdx_1)
00000b9b      puts(str: &var_c8)
00000bb6      printf(format: "%s\x1b[0m\n", &var_e8)
00000bd1      printf(format: "\n\x1b[32m%s\x1b[0m\n", &var_108)
00000cb0  if ((rax ^ *(fsbase + 0x28)) == 0)
00000cb0      return 0
00000ca2  __stack_chk_fail()
```

We can verify this by first sending 32 `A` characters to verify that the program still works, and then sending 33 `A` characters to see the flag printed:

```bash
python -c "print('A'*32)" | ./rot13

===================================
ROT IN sHELL :: ROT13's your input!
===================================
>
ROT13 output:
> NNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNN
```

```bash
python -c "print('A'*33)" | ./rot13

===================================
ROT IN sHELL :: ROT13's your input!
===================================
>
Segmentation fault.
*** stack smashing detected ***: <unknown> terminated
Aborted (core dumped)

Flag: luckyNumber13
```

### Flag: luckyNumber13