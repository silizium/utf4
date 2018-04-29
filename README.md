# utf4
Implementation of my weekend project, a 4-bit based UTF8 encoder (in Lua, but other languages might follow)

## WTF why? Dafaq?

Well, I thought it would be nice to have an UTF encoder that works with nibbles as its natural size, so that if I put the typical most used ENIRSAT plus Space into the first eight characters of the first nibble, I could well, reduce the size of any transmitted text over a bandwidth by great means, without actually needing to pack them. 

I came up with that silly idea, because I was working on code for my students, who happen to do classic cryptology at the time and I ased myself if that was possible. Because I'm at the moment mainly playing around with the idea more than having a product, I hacked that thing quickly in Lua.

## Test

```
> echo Er stand auf seines Daches Zinnen und schaute mit vergnügten Sinnen auf das beherrschte Samos hin: \"dies alles ist mir untertänig\", begann er zu Ägyptens König, \"gestehe, daß ich glücklich bin.\"|./pipe_utf4.lua|tee >(xxd >/dev/stderr)|./pipe_utf4.lua -d  |tee >(xxd >/dev/stderr)   
00000000: 8d50 4067 a200 d6b1 0014 2341 c008 96d0  .P@g......#A....
00000010: 1004 ba30 2221 d021 0a40 090d d671 0119  ...0"!.!.@...q..
00000020: 73e0 11c5 20fb c170 21b0 0a23 1202 d6b1  s... ..p!..#....
00000030: 000a 4680 100d 5145 090d 17b0 0a96 a141  ..F...QE.......A
00000040: d030 f202 6f0a 1304 8681 1104 4307 1953  .0..o.......C..S
00000050: d021 1775 cb21 c3f0 c602 08c1 6022 1005  .!.u.!......`"..
00000060: 2a1d c018 0c29 1b17 42b0 09ed 21c3 c002  *....)..B...!...
00000070: 6f0c 4117 0dc1 020a e61b 3009 0dc0 80b1  o.A.......0.....
00000080: 1f09 0f18 93d0 0008 232b 6f7a            ........#+oz
Er stand auf seines Daches Zinnen und schaute mit vergnügten Sinnen auf das beherrschte Samos hin: "dies alles ist mir untertänig", begann er zu Ägyptens König, "gestehe, daß ich glücklich bin."
00000000: 4572 2073 7461 6e64 2061 7566 2073 6569  Er stand auf sei
00000010: 6e65 7320 4461 6368 6573 205a 696e 6e65  nes Daches Zinne
00000020: 6e20 756e 6420 7363 6861 7574 6520 6d69  n und schaute mi
00000030: 7420 7665 7267 6ec3 bc67 7465 6e20 5369  t vergn..gten Si
00000040: 6e6e 656e 2061 7566 2064 6173 2062 6568  nnen auf das beh
00000050: 6572 7273 6368 7465 2053 616d 6f73 2068  errschte Samos h
00000060: 696e 3a20 2264 6965 7320 616c 6c65 7320  in: "dies alles 
00000070: 6973 7420 6d69 7220 756e 7465 7274 c3a4  ist mir untert..
00000080: 6e69 6722 2c20 6265 6761 6e6e 2065 7220  nig", begann er 
00000090: 7a75 20c3 8467 7970 7465 6e73 204b c3b6  zu ..gyptens K..
000000a0: 6e69 672c 2022 6765 7374 6568 652c 2064  nig, "gestehe, d
000000b0: 61c3 9f20 6963 6820 676c c3bc 636b 6c69  a.. ich gl..ckli
000000c0: 6368 2062 696e 2e22 0a                   ch bin.".
```

As you can see the textsize is reduced down to 69% of the original text, which I pipe out after decoding. So, it works

## todo

I still am in early development, I don't have my third codepage filled out completely, more than 300 characters are still free to go. And after this first step to implement most common characters up to the complete latin environmen of UTF, I am pondering how to easy implement the rest. 
