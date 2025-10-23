---
layout: page
title: Byte Order Mark - 3 invisible characters
---
Find files with BOM (Byte Order Mark) characters at the beginning of file:
```bash
for f in $(find . -type f); do if [ "$(head -n 1 ${f} | hexdump -C | grep "ef bb bf" | wc -l)" == "1" ]; then echo ${f}; fi; done
```
This is how a BOM file looks like:
```bash
[root@cvst-pgrozav aaa]# head -n 1 ./my.cpp | hexdump -C
00000000  ef bb bf 23 69 6e 63 6c  75 64 65 20 3c 63 68 72  |...#include <chr|
00000010  6f 6e 6f 3e 0a                                    |ono>.|
00000015
```

The characters are HEX: "ef bb bf" (3 invisible characters):

Fix, remove the characters:
```bash
sed -i 's/^\xef\xbb\xbf//g' ./my.cpp
```
