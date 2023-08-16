```bash
radare2 helloworld.o
```

We can search by typing: `/ thingwewannasearch`

```radare2
[0x00001040]> / Hello  
Searching 5 bytes in [0x4018-0x4020]  
hits: 0  
Searching 5 bytes in [0x3dd0-0x4018]  
hits: 0  
Searching 5 bytes in [0x2000-0x20b4]  
hits: 1  
Searching 5 bytes in [0x1000-0x1161]  
hits: 0  
Searching 5 bytes in [0x0-0x630]  
hits: 0  
0x00002004 hit1_0 .Hello world!; .
```

Than we can use that to set the cursor to the address where the string starts (the seeker in radare terms), in this case `0x00002004` with `s 0x00002004`, then we can look at the file contents with `px`

```
[0x00001040]> s 0x00002004  
[0x00002004]> px  
- offset -   4 5  6 7  8 9  A B  C D  E F 1011 1213  456789ABCDEF0123  
0x00002004  4865 6c6c 6f20 776f 726c 6421 0000 0000  Hello world!....  
0x00002014  011b 033b 2000 0000 0300 0000 0cf0 ffff  ...; ...........  
0x00002024  5400 0000 2cf0 ffff 3c00 0000 25f1 ffff  T...,...<...%...  
0x00002034  7c00 0000 1400 0000 0000 0000 017a 5200  |............zR.  
0x00002044  0178 1001 1b0c 0708 9001 0000 1400 0000  .x..............  
0x00002054  1c00 0000 e8ef ffff 2600 0000 0044 0710  ........&....D..  
0x00002064  0000 0000 2400 0000 3400 0000 b0ef ffff  ....$...4.......  
0x00002074  2000 0000 000e 1046 0e18 4a0f 0b77 0880   ......F..J..w..  
0x00002084  003f 1a3b 2a33 2422 0000 0000 1c00 0000  .?.;*3$"........  
0x00002094  5c00 0000 a1f0 ffff 1a00 0000 0041 0e10  \............A..  
0x000020a4  8602 430d 0655 0c07 0800 0000 0000 0000  ..C..U..........  
0x000020b4  ffff ffff ffff ffff ffff ffff ffff ffff  ................  
0x000020c4  ffff ffff ffff ffff ffff ffff ffff ffff  ................  
0x000020d4  ffff ffff ffff ffff ffff ffff ffff ffff  ................  
0x000020e4  ffff ffff ffff ffff ffff ffff ffff ffff  ................  
0x000020f4  ffff ffff ffff ffff ffff ffff ffff ffff  ................
```

Then we use `o++` to reopen the file in write mode, and write with `w hola, mundo\x00`
Then quit with `q`

```radare
[0x00002004]> oo+  
[0x00002004]> w hola, mundo\x00  
[0x00002004]> q

❯ ./helloworld.o    
hola, mundo
```
