---
layout: page
title: Binary files
---

Keep in mind the [ASCII table](https://www.asciitable.com/).

Writing characters 'a' and 'b' (with numeric values / ASCII codes: 97 for 'a' and 98 for 'b') and looking at it with different tools:
```bash
# view characters as base 2
paul@server:~$ echo -n "ab" | xxd -b
00000000: 01100001 01100010                                      ab
# The xxd tool prints the value of each character as an 8-bits binary value
# so 01100001 <=> 97 and 01100010 <=> 98

# view characters as base 10
paul@server:~$ echo -n "ab" | od -An -t uC
  97  98
# The od tool prints the character values as base 10 (decimal)

# view characters as base 16
paul@server:~$ echo -n "ab" | hexdump -C
00000000  61 62                                             |ab|
# The hexdump tool prints the character values as base 16 (hex)
# so 61 <=> 6*16 + 1 = 97   and   62 <=> 6*16 + 2 = 98
```

---

Printing characters "ab" based on their numeric value:
```bash
# generate characters from base 2
paul@server:~$ ( printf "\x$(bc <<< "ibase=2;obase=10000;01100001")"   &&   printf "\x$(bc <<< "ibase=2;obase=10000;01100010")" )
ab
# The bc tool will take input 01100001 (character 97 = 'a') and interpret it
# as input_base = 2 and will print the value as output_base = 10000 (which
# is really base 16 but written as base 2, so 10000 <=> 16).
# So, it will take the base 2 values, convert them to base 16 and then print
# the characters

# generate characters from base 10
paul@server:~$ ( printf "\x$(printf %x 97)" && printf "\x$(printf %x 98)" )
ab

# generate characters from base 16
paul@server:~$ ( printf "\x61" && printf "\x62" )
ab
```
