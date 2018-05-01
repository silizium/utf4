# utf4
Implementation of my weekend project, a 4-bit based Unicode encoder (in Lua, but other languages might follow)

## WTF why? Dafaq?

Well, I thought it would be nice to have an UTF encoder that works with nibbles as its natural size, so that if I put the typical most used ENIRSAT plus Space into the first eight characters of the first nibble, I could well, reduce the size of any transmitted text over a bandwidth by great means, without actually needing to pack them. 

I came up with that silly idea, because I was working on code for my students, who happen to do classic cryptology at the time and I asked myself if that was possible. Because I'm at the moment mainly playing around with the idea more than having a product, I hacked that thing quickly in Lua. I mean, why not using all the bits we have and don't waste them?

Bandwidth isn't developing at all. We try to go higher up in the frequency range, but shortage of bandwidth when it comes to transmitting never changes, I just say: "SMS", if you know what I mean.

## What UTF4 does for me?

It is packing the character string by using only 4 bits for the most common characters, doing most other characters in 8 bits as usual and only very uncommon in 12 bits. It converts the first byte of normal UTF-8 codes into three nibbles, so normal UTF-8 gets usually 4 bits longer.

But the thing is, that the most common 8 letters in a language usually fill more than 60% of the language, so the compressing effect is quite huge, without costing any serious calculation time, or transmitting the stuff in blocks or so. UTF4 is a stream encoding, that makes it possible to transmit character after character (if the stream is concurrent, of course, and not collecting blocks itself). 

You can use UTF4 everywhere, where you need very small bandwidth and storage space and if nobody knows how this encoding works, hell, it could even be taken as a weak sort of cipher! Whatever. I wanted to try if I could fill the bits of a papertape in an efficient way with text, like in the old days. I made it. This code is even more compact than CCITT#2/Murray or also known as Baudot code. So, if you want to bring down the space your text takes or you want to make your students mad trying to crack this code, go for it!

UTF4 supports _full_ UTF-8, so the letters it doesn't support in its internal table, it supports by converting it to normal UTF-8. All my text tests were about 60-65% smaller than normal UTF-8. That can be quite an useful, I guess.

## Test

```
> echo Er stand auf seines Daches Zinnen und schaute mit vergnügten Sinnen auf das beherrschte Samos hin: „dies alles ist mir untertänig”, begann er zu Ägyptens König, „gestehe, daß ich glücklich bin.”|./pipe_utf4.lua|tee >(xxd >/dev/stderr)|./pipe_utf4.lua -d  |tee >(xxd >/dev/stderr)
00000000: 7b05 7426 0a60 1d0b 4031 1204 7996 d010  {.t&.`..@1..y...
00000010: 04e9 3022 21d0 210a 4009 0dd6 7101 1973  ..0"!.!.@...q..s
00000020: e011 c520 e8c2 7021 a037 2221 601d 0ba0  ... ..p!.7"!`...
00000030: 6004 08d1 1055 94d0 7001 7a96 a141 d030  `....U..p.z..A.0
00000040: f202 9ca1 3041 6018 1841 3074 9031 051d  ....0A`..A0t.1..
00000050: 7251 872b 320c 9dc1 0208 c160 2210 052a  rQ.+2......`"..*
00000060: 1d90 1f0c 291b 1742 e0a7 2d32 0c2c c019  ....)..B..-2.,..
00000070: 0c41 170d c102 0ab6 2a30 090d c080 812e  .A......*0......
00000080: 090f 1893 d000 0823 2b9d a109            .......#+...
Er stand auf seines Daches Zinnen und schaute mit vergnügten Sinnen auf das beherrschte Samos hin: „dies alles ist mir untertänig”, begann er zu Ägyptens König, „gestehe, daß ich glücklich bin.”
00000000: 4572 2073 7461 6e64 2061 7566 2073 6569  Er stand auf sei
00000010: 6e65 7320 4461 6368 6573 205a 696e 6e65  nes Daches Zinne
00000020: 6e20 756e 6420 7363 6861 7574 6520 6d69  n und schaute mi
00000030: 7420 7665 7267 6ec3 bc67 7465 6e20 5369  t vergn..gten Si
00000040: 6e6e 656e 2061 7566 2064 6173 2062 6568  nnen auf das beh
00000050: 6572 7273 6368 7465 2053 616d 6f73 2068  errschte Samos h
00000060: 696e 3a20 e280 9e64 6965 7320 616c 6c65  in: ...dies alle
00000070: 7320 6973 7420 6d69 7220 756e 7465 7274  s ist mir untert
00000080: c3a4 6e69 67e2 809d 2c20 6265 6761 6e6e  ..nig..., begann
00000090: 2065 7220 7a75 20c3 8467 7970 7465 6e73   er zu ..gyptens
000000a0: 204b c3b6 6e69 672c 20e2 809e 6765 7374   K..nig, ...gest
000000b0: 6568 652c 2064 61c3 9f20 6963 6820 676c  ehe, da.. ich gl
000000c0: c3bc 636b 6c69 6368 2062 696e 2ee2 809d  ..cklich bin....
000000d0: 0a 
```

As you can see the textsize is reduced down to about 63% of the original text, which I pipe out after decoding. So, it works

## Dependency

Trickle: https://github.com/bjornbytes/trickle

## todo

Doint the stuff in "C".
