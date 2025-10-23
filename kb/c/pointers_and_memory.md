---
layout: page
title: Pointers and memory
---

### 1. Values and data types

Luckily, C and C++ only support integer basic data types. That means that every
basic value is a number in C++. Maybe in contrast with what we're expecting,
strings and characters are also numbers. You can see the relation between
decimal numbers and characters by looking at the ASCII table for example at:
<a href="https://www.asciitable.com/" target="_blank">https://www.asciitable.com/
</a> .
<br/><br/>
Of course, there are also composite data types such as structures and classes,
but I'll not cover that in this text.
<br/><br/>
So, let's get back to the basics, there are multiple types of numbers that C/C++
supports. The difference between them is: the size(how big the numbers can be -
how many digits) and the sign(whether or not the number can be negative or just
positive).
<br/><br/>
Yes, there are also floating point numbers, but I'm not going to cover them
either, I'll just stick with the basics here.
<br/><br/>
So, looking at the numeric types by size, we have 4 sizes. 1-byte integer
numbers, 2-bytes int numbers, 4 bytes and 8 bytes. Yes that's powers of 2. There
are no 3-bytes integer numbers. Though that's an interesting topic, maybe I'll
cover it in another text. Now, each of these 4 integer types, have a signed and
unsigned type. So, that means that a 1-byte integer can be signed (will contain
the sign inside it, so you can have negative values like -5 and positive values
like +10), and, also, the 1-byte integer can be unsigned(will not contain a
sign, thus, it can only store positive values: 0, 1, 2, 3, ...). See more on
this by looking at the <a href="/kb/c/numeric_limits" target="_blank">numeric
limits</a> page.
<br/><br/>
The 1-byte integer numeric data type is also called `char`. That's because it is
often used to store a character - even though it can only store numeric values.
But looking at the ASCII table you can see that the numeric positive value 65
is the equivalent of character `A`. The numeric positive (integer) value of 97
is the equivalent of character `a` (note that upper-case and lower-case
characters have a different number associated with them). Even the digit
characters have a numeric equivalent. So for example the character `7` has a
numeric value of 55. There's a number even for punctuation characters - for
example the caracter `;` has a numeric value of 59, the space character(" ") has
a value of 32 - and to make things even more complicated, there are even
"invisible" or "non printable characters" like the numeric value 10 which is
reserved for the new-line character, or number 27 which is reserved for the
Escape character. But, you can see all the characters in the ASCII table and the
extended-ASCII table.
<br/><br/>
I should mention, however, that while you store numeric values in a `char`
variable, when you print it on the screen, it will actually print the equivalent
character. That is in contrast with 2-byte integers for example (known as
`short int`):
```cpp
paul@alice:~$ cat a.cpp
#include <iostream>
using namespace ::std;
int main(){
  char a = 98; // This is the numeric value for character 'b'
  cout << a << endl;
  return 0;
}
paul@alice:~$ g++ a.cpp -o a.exe && ./a.exe
b

# ============================================================================ #

paul@alice:~$ cat a.cpp
#include <iostream>
using namespace ::std;
int main(){
  // The 'd' character is "converted" to it's numeric equivalent of 100 and
  // the value 100 is stored inside the c variable
  char c = 'd';
  // but again, when printed, it will "convert" the number back into a character
  cout << c << endl;

  // this is also character 'd' but expressed as a number
  char e = 100;
  // this will produce the same output as above.
  cout << e << endl;
  return 0;
}
paul@alice:~$ g++ a.cpp -o a.exe && ./a.exe
d
d

# ============================================================================ #

paul@alice:~$ cat a.cpp
#include <iostream>
using namespace ::std;
int main(){
  // this value of 100 is stored as a number in the f variable
  short int f = 100;
  // when printed, we'll see the numeric value of 100, just what we asigned.
  cout << f << endl;

  // The 'd' character is "converted" to it's numeric equivalent of 100 and
  // the value 100 is stored inside the g variable
  short int g = 'd';
  // this will produce the same output as above. The numeric value is printed.
  cout << g << endl;
  return 0;
}
paul@alice:~$ g++ a.cpp -o a.exe && ./a.exe
100
100

# ============================================================================ #

paul@alice:~$ cat a.cpp
#include <iostream>
using namespace ::std;
int main(){
  // The character values are actual numbers, you can:
  short int h = 'd' - 'D' - 10;
  // which means 'd' = 100 , 'D' = 68 and 10 = 10
  // so h = 100 - 68 - 10 = 22
  // so the numeric value 22 will be printed.
  cout << h << endl;

  return 0;
}
paul@alice:~$ g++ a.cpp -o a.exe && ./a.exe
22
```

So, 2, 4 and 8 bytes integer data types print the numeric value that they hold,
it is just the 1-byte integers that are printed as their corresponding character
value.
<br/><br/>
Also, note, that strings, or sequences of characters can not be stored into a
`char` data type. We'll have distinct data types like `char *` for C-strings and
`::std::string` for C++ strings - but more on that, later. However, when writing
string literals, you should note that the use of single quotes(`''`) is reserved
for one single character, and the use of double quotes(`""`) is reserved for
strings(with zero, one or multiple characters). So, `'A'` is valid but `'this'`
is not valid. However `"this"` is valid, `"T"` is also valid, and `""` is also
valid.
<br/><br/>
Here's a little example that will not work as expected:
```cpp
paul@alice:~$ cat a.cpp
#include <iostream>
using namespace ::std;
int main(){
  // 1. the compiler can understand this, it's the character A
  char a = 'A';
  cout << a << endl;

  // 2. the compiler will complain about this. The use of single quote tells
  // the compiler that a character value is following, but, instead, it finds
  // multiple characters: t, h, i and s. So, this will result in a compilation
  // warning and also, the program will probably not work as expected. More on
  // this, below.
  char b = 'this';
  cout << b << endl;

  // 3. What's with the weird asterisk (*) ?
  // Anyway, the use of double quotes, tells the compiler that a sequence of
  // characters (string) is following, so: t, h, i and s are valid characters
  // and the sequence ends when another (non-escaped) double-quote character
  // is met. So, this seems valid, from the compiler's perspective, no complains
  // here.
  char *c = "this";
  cout << c << endl;

  // 4. Also fine, a sequence of characters is valid even if it contains one
  // character ...
  char *d = "A";
  cout << d << endl;

  // 5. Also fine, a sequence of characters is valid even if it contains no
  // characters at all ...
  char *e = "";
  cout << e << endl;
  return 0;
}
paul@alice:~$ g++ a.cpp -o a.exe
a.cpp:13:12: warning: multi-character character constant [-Wmultichar]
   13 |   char b = 'this';
      |            ^~~~~~
a.cpp: In function ‘int main()’:
a.cpp:13:12: warning: overflow in conversion from ‘int’ to ‘char’ changes value from ‘1952999795’ to ‘'s'’ [-Woverflow]
a.cpp:22:13: warning: ISO C++ forbids converting a string constant to ‘char*’ [-Wwrite-strings]
   22 |   char *c = "this";
      |             ^~~~~~
a.cpp:27:13: warning: ISO C++ forbids converting a string constant to ‘char*’ [-Wwrite-strings]
   27 |   char *d = "A";
      |             ^~~
a.cpp:32:13: warning: ISO C++ forbids converting a string constant to ‘char*’ [-Wwrite-strings]
   32 |   char *e = "";
      |             ^~
paul@alice:~$ ./a.exe
A
s
this
A

paul@alice:~$
```

The compilation succeded, but it threw some warnings. The first 5 lines of
warnings are the most interesting.
<br/><br/>
You can see that the program output is `A` when printing `char a`, then for `char b`
only an `s` is printed, instead of `this`. There was even a warning for this,
and as I said, the program will not work as expected, that character data type
can not hold more than one character (numeric value). Then for `char *` c, d and
e the output is what we expect, the value that was assigned.










<br/><br/>
<br/><br/>

### 2. We hold values in memory.
You can imagine memory as a sequence of bytes(memory cells) that are indexed:

<style>
.memory_table {
  table-layout: fixed;
}
.memory_table, td {
  border-collapse: collapse;
  border: 1px solid black;
  text-align: center;
  width: 25px;
  height: 25px;
}
</style>
<table class="memory_table">
  <tr>
    <td style="width: 180px;">memory byte <b>index</b></td>
    <td>0</td>
    <td>1</td>
    <td>2</td>
    <td>3</td>
    <td>4</td>
    <td>5</td>
    <td>6</td>
    <td>7</td>
    <td>8</td>
    <td>...</td>
  </tr>
  <tr>
    <td style="width: 180px;">memory byte <b>value</b></td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td>...</td>
  </tr>
</table>

Just like a vector, or an array. But remember that it has a lot of cells. If
your computer has 4 GiB of RAM, then it means that your memory has 4 294 967 296
cells, that's more than 4 billion cells.
<br/><br/>
So, for example if we want to place the value `82` at index 7, let's say, then,
the memory would look like this:

<table class="memory_table">
  <tr>
    <td style="width: 180px;">memory byte <b>index</b></td>
    <td>0</td>
    <td>1</td>
    <td>2</td>
    <td>3</td>
    <td>4</td>
    <td>5</td>
    <td>6</td>
    <td>7</td>
    <td>8</td>
    <td>...</td>
  </tr>
  <tr>
    <td style="width: 180px;">memory byte <b>value</b></td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td>82</td>
    <td></td>
    <td>...</td>
  </tr>
</table>
<br/>
Remember that each cell is one byte, or we can see it as a `char` data-type. So,
we can also consider that at cell index `7` we have a value of `R` ( `82` is the
numeric value for the `R` character, according to the ASCII table ):

<table class="memory_table">
  <tr>
    <td style="width: 180px;">memory byte <b>index</b></td>
    <td>0</td>
    <td>1</td>
    <td>2</td>
    <td>3</td>
    <td>4</td>
    <td>5</td>
    <td>6</td>
    <td>7</td>
    <td>8</td>
    <td>...</td>
  </tr>
  <tr>
    <td style="width: 180px;">memory byte <b>value</b></td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td>R</td>
    <td></td>
    <td>...</td>
  </tr>
</table>

Now, let's change the terminology a little bit. The memory byte `index` is often
called the memory `address`, because it tells you where a certain value is
placed in the memory. Just like an address tells you where a person lives in a
city. Remember that every time we refer to a "memory address", that's just the
index of a byte in the memory. So, a memory address is, in the end, just a
natural number, ..., an integer number that is >= 0 .

<table class="memory_table">
  <tr>
    <td style="width: 180px;">memory byte <b>address</b></td>
    <td>0</td>
    <td>1</td>
    <td>2</td>
    <td>3</td>
    <td>4</td>
    <td>5</td>
    <td>6</td>
    <td>7</td>
    <td>8</td>
    <td>...</td>
  </tr>
  <tr>
    <td style="width: 180px;">memory byte <b>value</b></td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td>R</td>
    <td></td>
    <td>...</td>
  </tr>
</table>










<br/><br/>
<br/><br/>

### 3. Variables in memory

So, what happens when we <b>define</b> a variable?
```cpp
  char a = 4;
```
This is a <b>variable</b> named `a`, which holds/contains a value of `4`.
Because the data type of this variable is `char`, that means that it's value is
stored in 1 byte of memory(remember that char is 1-byte sized numerical integer
data type?). So, that means that <b>somewhere</b> inside the computer's memory
, at some <b>unknown</b> address, there is a cell, a byte, that has (was
assigned) a value of `4`.

<table class="memory_table">
  <tr>
    <td style="width: 180px;">memory byte <b>address</b></td>
    <td>...</td>
    <td style="width: 50px;">17628</td>
    <td style="width: 50px;">17629</td>
    <td style="width: 50px;">17630</td>
    <td style="width: 50px;">17631</td>
    <td style="width: 50px;">17632</td>
    <td style="width: 50px;">17633</td>
    <td style="width: 50px;">17634</td>
    <td style="width: 50px;">17635</td>
    <td>...</td>
  </tr>
  <tr>
    <td style="width: 180px;">memory byte <b>value</b></td>
    <td>...</td>
    <td></td>
    <td></td>
    <td></td>
    <td style="background-color: red;">4</td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td>...</td>
  </tr>
</table>
In this case, the value `4` is located at the memory address `17631`.
<br/><br/>
Then, later, if we do:
```cpp
a = 21;
```
The value in that memory cell/byte is going to be updated/changed to reflect the
new value that was assigned to variable `a`. So, after that instruction, the
memory would look like:
<table class="memory_table">
  <tr>
    <td style="width: 180px;">memory byte <b>address</b></td>
    <td>...</td>
    <td style="width: 50px;">17628</td>
    <td style="width: 50px;">17629</td>
    <td style="width: 50px;">17630</td>
    <td style="width: 50px;">17631</td>
    <td style="width: 50px;">17632</td>
    <td style="width: 50px;">17633</td>
    <td style="width: 50px;">17634</td>
    <td style="width: 50px;">17635</td>
    <td>...</td>
  </tr>
  <tr>
    <td style="width: 180px;">memory byte <b>value</b></td>
    <td>...</td>
    <td></td>
    <td></td>
    <td></td>
    <td style="background-color: red;">21</td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td>...</td>
  </tr>
</table>
<br/>
So, then, we could say that a <b>variable</b> is a name that we use to refer to
a certain memory space(one or multiple cells/bytes) that can hold a value. So,
in a way, we can say that the variable <b>is</b> that memory space(cells) that
hold/contain the value, but we refer to that space using a name, a variable
name.
<br/><br/>
Remember that we can also have variables that have a data-type that is more than
1 byte in size. For example:
```cpp
  // this requires 1 byte of memory (blue)
  char a = 12;

  // this requires 2 bytes of memory (red)
  short int b = 11;

  // this requires 4 bytes of memory (yellow)
  int c = 10;

  // this requires 8 bytes of memory (green)
  long long int d = 9;
```
So, after defining these variables, our memory might look like:
<table class="memory_table">
  <tr>
    <td style="width: 180px;">memory byte <b>address</b></td>
    <td>0</td>
    <td>1</td>
    <td>2</td>
    <td>3</td>
    <td>4</td>
    <td>5</td>
    <td>6</td>
    <td>7</td>
    <td>8</td>
    <td>9</td>
    <td>10</td>
    <td>11</td>
    <td>12</td>
    <td>13</td>
    <td>14</td>
    <td>15</td>
    <td>16</td>
    <td>17</td>
    <td>18</td>
    <td>19</td>
    <td>...</td>
  </tr>
  <tr>
    <td style="width: 180px;">memory byte <b>value</b></td>
    <td></td>
    <td style="background-color: blue;">12</td>
    <td></td>
    <td style="background-color: red;" colspan="2">11</td>
    <td></td>
    <td style="background-color: yellow;" colspan="4">10</td>
    <td></td>
    <td style="background-color: green;" colspan="8">9</td>
    <td></td>
    <td>...</td>
  </tr>
</table>
I'm using smaller index numbers, to save some space
<br/><br/>
But let's look at the indexes in a "real world" example. But can we know
<b>where</b> inside the memory, the computer will place our value? Can we know
the <i>address</i> of our variable `a`? Well, it turns out we can. The address
of variable `a` is: `&a`:
```cpp
paul@alice:~$ cat a.cpp
#include <iostream>
using namespace ::std;
int main() {
  // save the value 4 at SOME address in memory
  short int a = 4;
  // we print the index/position where the value 4 is in memory:
  cout << &a << endl;
  return 0;
}
paul@alice:~$ g++ a.cpp -o a.exe && ./a.exe
0x7ffda8568af6
```
Whaaat? `0x7ffda8568af6` ? How is that a number? How is that the position in the
memory array? Shouldn't it be a number like 1 or 1 000 000 or something, but
still a number? This contains characters like `x`, `d`, `a`, `f`, surely this
isn't a number ... or is it?
<br/><br/>
Well, actually, this <b>is</b> a number! It's just that it's not in the decimal
form, as we are used to see the numbers. The prefix `0x` tells us that this is
a <a href="https://en.wikipedia.org/wiki/Hexadecimal" target="_blank">
hexadecimal</a> (base 16) number. The actual number is `7ffda8568af6`. But why
does the computer show the memory address as a hexadecimal number instead of
using the regular decimal numbering system, that we are used with? Well, if it
would show it as a decimal, the address would be `140 727 427 697 398`. Memory
addresses are printed in hexadecimal just to be shorter. The most common thing
we do with these values is to compare them, so having fewer digits/characters,
they are easier to compare.
<br/><br/>
Now, let's represent this memory as a table:
<table class="memory_table">
  <tr>
    <td style="width: 180px;">memory byte <b>address</b></td>
    <td>...</td>
    <td style="width: 175px;">140 727 427 697 397</td>
    <td style="width: 175px;">140 727 427 697 398</td>
    <td style="width: 175px;">140 727 427 697 399</td>
    <td style="width: 175px;">140 727 427 697 400</td>
    <td>...</td>
  </tr>
  <tr>
    <td style="width: 180px;">memory byte <b>value</b></td>
    <td>...</td>
    <td></td>
    <td style="background-color: red;" colspan="2">4</td>
    <td></td>
    <td>...</td>
  </tr>
</table>
Because our variable has a data type of `short int`, which has a size of 2
bytes, our value will require 2 cells to be stored. And the address of the
variable (the index) is `140 727 427 697 398`. You should know that the address
of a variable will always return <b>the index of the first byte/cell in that
memory space</b> occupied by that variable. So our variable uses the bytes at
index `140 727 427 697 398` and `140 727 427 697 399`.

You should know that if you run the program above, you'll probably not get the
same value of `0x7ffda8568af6` as I did. Well, even if I run the same program
again, I'll not get the same value at the second run. That is because every time
a program starts, it stores it's variables in a different memory space.
```cpp
paul@alice:~$ ./a.exe
0x7ffed2a45166
paul@alice:~$ ./a.exe
0x7ffd2fa24d86
paul@alice:~$ ./a.exe
0x7ffed7e62f76
paul@alice:~$ ./a.exe
0x7ffd135e71e6
```
So, the same program places the value `4` in different memory spaces for each
run. However, while the program is running, if the value was placed at address
`0x7ffd135e71e6`, it will stay there until the program ends running or until the
value is overwritten.









<br/><br/>
<br/><br/>

### 4. Pointers

Well, now that you know what a <b>memory address</b> is, I can tell you that a
<b>pointer</b> is a variable that holds a memory address.

Being that a memory address is just a number (it might be big - but it's just a
number), we should be able to store it in a variable, using for example the
largest numerical data type, which uses 8 bytes - `long long int`:
```cpp
  long long int b = 140727427697398;
```
I'm just taking the value `140 727 427 697 398` from our pointer example above.
And, I'm assigning it to a `long long int` variable, to store it as a pointer
(number).
<br/><br/>
We can also write the same thing using a hexadecimal notation:
```cpp
  long long int b = 0x7ffda8568af6;
```
The C and C++ compiler knows that that is a hexadecimal value, and the variable
will have the same numeric value as in the example above. So, this compiles just
fine. It doesn't matter if we use the decimal or hexadecimal notation.
<br/><br/>
So, even though `long long int b` above, can be seen as a "pointer", because it
contains the memory address of our `short int a = 4;` (see the example above),
it is missing something that a normal pointer has. A pointer should tell us the
data type it points to. So instead of saving that address in a `long long int`,
we should save it into a data type: `short int *`, because that's the address of
a variable of type `short int`. So, the code should look like:
```cpp
  short int * b = 0x7ffda8568af6;
```
So, yes, `short int *` is actually a valid data type in C and C++, the code
compiles just fine.
<br/><br/>
Actually, for any data type `X`, there exists a data type `X *` which should be
interpreted as a pointer to an object of type `X` (a number, the index of the
first byte of memory where the object `X` is placed).
<br/><br/>
You should also know that a pointer variable also requires memory space to store
it's value (the value being the address of the variable/data it points to). But,
the size of any pointer data type, the number of bytes required to store a
memory address in memory, is fixed. This size does not depend on the size of the
object it points to.
<br/><br/>
So, for example a `char` requires 1 byte of memory to store it's value. But a
`char *` requires 8 bytes of memory to store it's value(the memory address). Or,
a `int` requires 4 bytes of memory to store it's value - but a `int *` requires
8 bytes of memory to store it's value. So, a pointer will always require the
same amount of bytes to store it's value, regardless of the data type it points
to.
<br/><br/>
So, generally speaking, a pointer `X *` will require 8 bytes of memory if the
application is running on 64-bit systems, and 4 bytes of memory on 32-bit
systems. So, on 64-bit systems, a pointer uses 64 bits of memory, and it uses 32
bits of memory on 32-bit hardware systems.
<br/><br/>
So, let's take a look at a pointer in memory. So we have a 2-byte integer named
`a` and a pointer to it, named `b`. Note that the value of `b` is the address of
`a`, that is `&a`:
```cpp
  short int a = 1; // blue
  short int * b = &a; // red
```
I'm using small index values / addresses to keep it simple:
<table class="memory_table">
  <tr>
    <td style="width: 180px;">memory byte <b>address</b></td>
    <td>0</td>
    <td>1</td>
    <td>2</td>
    <td>3</td>
    <td>4</td>
    <td>5</td>
    <td>6</td>
    <td>7</td>
    <td>8</td>
    <td>9</td>
    <td>10</td>
    <td>11</td>
    <td>12</td>
    <td>13</td>
    <td>14</td>
    <td>...</td>
  </tr>
  <tr>
    <td style="width: 180px;">memory byte <b>value</b></td>
    <td></td>
    <td></td>
    <td></td>
    <td style="background-color: blue;" colspan="2">1</td>
    <td></td>
    <td style="background-color: red;" colspan="8">3</td>
    <td></td>
    <td>...</td>
  </tr>
</table>
So, our pointer uses 8 bytes of memory(because I'm on a 64-bit machine), and
contains the index of the first byte of variable `a`. And that index is `3`, so,
`3` is the value of the pointer `b`, stored on 8 bytes in memory.
<br/><br/>
So, let's see this in action:
```cpp
paul@alice:~$ cat a.cpp
#include <iostream>
using namespace ::std;
int main() {
  short int a = 1;
  short int * b = &a;
  cout << "a=" << a << endl;
  cout << "b=" << b << endl;
  return 0;
}
paul@alice:~$ g++ a.cpp -o a.exe && ./a.exe
a=1
b=0x7ffcb44d739e
```
I hope the output makes sense.










<br/><br/>
<br/><br/>

### 5. Pointers to pointers
Going on with this example, we could also have a variable `c` which would be a
pointer to `b`.
```cpp
  short int a = 1; // blue
  short int * b = &a; // red
  short int * * c = &b; // green
```
So, the data type `short int * *` is a pointer to a variable of data type `short
int *`. So, you can have a pointer to a pointer, to a pointer, to a pointer, ...
, to a pointer, to a variable of type `short int`. So, that would be represented
in code, using as many asterisks(`*`) as needed.
<table class="memory_table">
  <tr>
    <td style="width: 180px;">memory byte <b>address</b></td>
    <td>0</td>
    <td>1</td>
    <td>2</td>
    <td>3</td>
    <td>4</td>
    <td>5</td>
    <td>6</td>
    <td>7</td>
    <td>8</td>
    <td>9</td>
    <td>10</td>
    <td>11</td>
    <td>12</td>
    <td>13</td>
    <td>14</td>
    <td>15</td>
    <td>16</td>
    <td>17</td>
    <td>18</td>
    <td>19</td>
    <td>20</td>
    <td>21</td>
    <td>22</td>
    <td>23</td>
    <td>...</td>
  </tr>
  <tr>
    <td style="width: 180px;">memory byte <b>value</b></td>
    <td></td>
    <td></td>
    <td></td>
    <td style="background-color: blue;" colspan="2">1</td>
    <td></td>
    <td style="background-color: red;" colspan="8">3</td>
    <td></td>
    <td style="background-color: green;" colspan="8">6</td>
    <td></td>
    <td>...</td>
  </tr>
</table>
So, our `c` pointer uses 8 bytes of memory, and contains the value `6` because
it points to variable `b`, which starts in memory at index `6`.
<br/><br/>
So, let's see this in action:
```cpp
paul@alice:~$ cat a.cpp
#include <iostream>
using namespace ::std;
int main() {
  short int a = 1;
  short int * b = &a;
  short int * * c = &b;
  cout << "a=" << a << endl;
  cout << "b=" << b << endl;
  cout << "c=" << c << endl;
  return 0;
}
paul@alice:~$ g++ a.cpp -o a.exe && ./a.exe
a=1
b=0x7ffe2c240606
c=0x7ffe2c240608
```
I hope the output makes sense.









<br/><br/>
<br/><br/>

### 6. Dereferencing pointers
Now, that you've learned (in the previous chapters) the `&` (ampersand)
operator, which, when applied to a variable, returns the memory address where
the value is located in the memory. Now, I can introduce the `*` (asterisk)
operator, which, when applied to a pointer variable, returns the <b>value</b>
from that memory address(that the pointer points to).
```cpp
paul@alice:~$ cat a.cpp
#include <iostream>
using namespace ::std;
int main() {
  short int a = 1;
  short int * b = &a;
  cout << "a=" << a << endl;
  cout << "b=" << b << endl;
  cout << "*b=" << *b << endl;
  return 0;
}
paul@alice:~$ g++ a.cpp -o a.exe && ./a.exe
a=1
b=0x7fff498c438e
*b=1
```
If `b` is a pointer to `a`, then `*b` is the value of `a`.
<br/><br/>
So, let's picture this:
```cpp
  short int a = 1; // blue
  short int * b = &a; // red
```
<table class="memory_table">
  <tr>
    <td style="width: 180px;">memory byte <b>address</b></td>
    <td>0</td>
    <td>1</td>
    <td>2</td>
    <td>3</td>
    <td>4</td>
    <td>5</td>
    <td>6</td>
    <td>7</td>
    <td>8</td>
    <td>9</td>
    <td>10</td>
    <td>11</td>
    <td>12</td>
    <td>13</td>
    <td>14</td>
    <td>...</td>
  </tr>
  <tr>
    <td style="width: 180px;">memory byte <b>value</b></td>
    <td></td>
    <td></td>
    <td></td>
    <td style="background-color: blue;" colspan="2">1</td>
    <td></td>
    <td style="background-color: red;" colspan="8">3</td>
    <td></td>
    <td>...</td>
  </tr>
</table>
Asterisk (`*`) is an operator, a function. You give it the address(and type)
through a pointer, and it gives you back the value(of that given type) starting
at the given address.
<br/><br/>
So, in the case of:
```cpp
  cout << *b << endl;
```
we give it the address inside b (`3`) and the type (type of pointer -
`short int`) and it returns the value `1`, which is the `short int` value, which
starts in memory at address `3`.
<br/><br/>
So, the cout above would just print the value `1`, which is also the value of
`a`.
<br/><br/>
But, if we would then execute:
```cpp
  *b = 99;
```
Then the memory would look like this:
<table class="memory_table">
  <tr>
    <td style="width: 180px;">memory byte <b>address</b></td>
    <td>0</td>
    <td>1</td>
    <td>2</td>
    <td>3</td>
    <td>4</td>
    <td>5</td>
    <td>6</td>
    <td>7</td>
    <td>8</td>
    <td>9</td>
    <td>10</td>
    <td>11</td>
    <td>12</td>
    <td>13</td>
    <td>14</td>
    <td>...</td>
  </tr>
  <tr>
    <td style="width: 180px;">memory byte <b>value</b></td>
    <td></td>
    <td></td>
    <td></td>
    <td style="background-color: blue;" colspan="2">99</td>
    <td></td>
    <td style="background-color: red;" colspan="8">3</td>
    <td></td>
    <td>...</td>
  </tr>
</table>
So, my point is that, if you have a pointer to `a`, you can both read and write
to the value of `a`, through(using) the pointer.
<br/><br/>
You can see that I am reading and writing from/to the variable `a`, using the
pointer `b`:
```cpp
paul@alice:~$ cat a.cpp
#include <iostream>
using namespace ::std;
int main() {
  short int a = 1;
  short int * b = &a;
  cout << "a=" << a << endl;
  cout << "b=" << b << endl;
  cout << "*b=" << *b << endl;
  *b = 99;
  cout << "a=" << a << endl;
  cout << "b=" << b << endl;
  cout << "*b=" << *b << endl;
  return 0;
}
paul@alice:~$ g++ a.cpp -o a.exe && ./a.exe
a=1
b=0x7ffe599ddbfe
*b=1
a=99
b=0x7ffe599ddbfe
*b=99
```
You can also see that `a` starts with a value of `1`, and `*b` is also `1`,
because it points to `a`. But I change the value of `a` to `99`, using `*b`, and
that reflects when I print the value of `a`, and also the value of `*b`.
<br/><br/>
Note that the value of `b` does not change - it points to the same fixed index
where the value of `a` is in memory. Even though the value of `a` changes, the
memory space reserved for the variable `a` remains the same.
<br/><br/>
It is also important to note that we only have 2 variables: `a`, and the pointer
`b`. Note that `*b` <b>is not a copy</b> of the value of `a`, but it is the same
value. `*b` can be used as a replacement of `a`, you can do the same things with
it.
<br/><br/>
I should also say that there is a <b>generic</b> pointer, of type `void *`. This
data type is just a number, an address, it doesn't know what data type it points
to. So, in order to use it, you have to cast it into another data type, and then
dereference it.
<br/><br/>












<br/><br/>
<br/><br/>

### 7. Dereferencing a pointer to a pointer
So, let's dereference a pointer to a pointer :) ...
```cpp
paul@alice:~$ cat a.cpp
#include <iostream>
using namespace ::std;
int main() {
  short int a = 1;
  short int * b = &a;
  short int * * c = &b;
  cout << "a=" << a << endl;
  cout << "b=" << b << endl;
  cout << "*b=" << *b << endl;
  cout << "c=" << c << endl;
  cout << "*c=" << *c << endl;
  cout << "**c=" << **c << endl;
  *b = 98;
  cout << "a=" << a << endl;
  cout << "b=" << b << endl;
  cout << "*b=" << *b << endl;
  cout << "c=" << c << endl;
  cout << "*c=" << *c << endl;
  cout << "**c=" << **c << endl;
  (**c)++;
  cout << "a=" << a << endl;
  cout << "b=" << b << endl;
  cout << "*b=" << *b << endl;
  cout << "c=" << c << endl;
  cout << "*c=" << *c << endl;
  cout << "**c=" << **c << endl;

  return 0;
}
paul@alice:~$ g++ a.cpp -o a.exe && ./a.exe
a=1
b=0x7ffe1e60f986
*b=1
c=0x7ffe1e60f988
*c=0x7ffe1e60f986
**c=1
a=98
b=0x7ffe1e60f986
*b=98
c=0x7ffe1e60f988
*c=0x7ffe1e60f986
**c=98
a=99
b=0x7ffe1e60f986
*b=99
c=0x7ffe1e60f988
*c=0x7ffe1e60f986
**c=99
```
So, let's digest this:
<br/>
`a` has a value of `1` and an address of `0x7ffe1e60f986`.
<br/>
`b` has a value of `0x7ffe1e60f986` and an address of `0x7ffe1e60f988`.
<br/>
`c` has a value of `0x7ffe1e60f988` and an address that is unknown to us (we
didn't print it - though we could).
<br/><br/>
So, again, the value of `b` and the value of `c` does not change, so let's
remove that from the output to focus on what is important:
```cpp
a=1
*b=1
*c=0x7ffe1e60f986
**c=1
a=98
*b=98
*c=0x7ffe1e60f986
**c=98
a=99
*b=99
*c=0x7ffe1e60f986
**c=99
```
So, since `c` is a pointer to `b`, it's obvious that when you dereference `c`
(that is `*c`) you'll get the value of b, which is `0x7ffe1e60f986`. But again,
the value of `b` is constant, it points to the fixed address of `a`, so that's
why the value `*c` stays constant in the output. So let's remove it too:
```cpp
a=1
*b=1
**c=1
a=98
*b=98
**c=98
a=99
*b=99
**c=99
```
So, if we derefecence the `c` pointer, we obtain the `b` pointer. And if we
dereference the `b` pointer we obtain the `a` variable. That's why both `**c`
and `*b` are equal to `1` which is the value of `a` at the beginning.
<br/><br/>
Then we set the value of `a` to `98` through the `b` pointer, in instruction:
`*b = 98;` . This is reflected when reading it through the pointers `b` and `c`.
<br/><br/>
Then we increment the value of `a` to `99` through the `c` pointer, in
instruction: `(**c)++;`. This instruction takes the value `**c` (the value of
`a`) and increments it using the `++` operator. Note that I've used parentheses
to make it clear that the dereferencing operations should happen first, and only
then, on the final value(of `a`) we should apply the increment operator(`++`).
<br/><br/>
So, did you manage to picture the memory in your head? Let's do it together, but
using hexadecimal addresses instead, to keep it simpler:
<table class="memory_table">
  <tr>
    <td style="width: 180px;">memory byte <b>address</b></td>
    <td>...</td>
    <td style="width: 100px;">7ffe1e60f9<b>85</b></td>
    <td style="width: 100px;">7ffe1e60f9<b>86</b></td>
    <td style="width: 70px;">...60f9<b>87</b></td>
    <td style="width: 70px;">...60f9<b>88</b></td>
    <td style="width: 70px;">...60f9<b>89</b></td>
    <td style="width: 70px;">...60f9<b>90</b></td>
    <td style="width: 70px;">...60f9<b>91</b></td>
    <td style="width: 70px;">...60f9<b>92</b></td>
    <td style="width: 70px;">...60f9<b>93</b></td>
    <td style="width: 70px;">...60f9<b>94</b></td>
    <td style="width: 70px;">...60f9<b>95</b></td>
    <td style="width: 70px;">...60f9<b>96</b></td>
    <td style="width: 70px;">...60f9<b>97</b></td>
    <td style="width: 70px;">...60f9<b>98</b></td>
    <td style="width: 70px;">...60f9<b>99</b></td>
    <td style="width: 70px;">...6100<b>00</b></td>
    <td style="width: 70px;">...6100<b>01</b></td>
    <td style="width: 70px;">...6100<b>02</b></td>
    <td style="width: 70px;">...6100<b>03</b></td>
    <td style="width: 100px;">7ffe1e6100<b>04</b></td>
    <td>...</td>
  </tr>
  <tr>
    <td style="width: 180px;">memory byte <b>value</b></td>
    <td>...</td>
    <td></td>
    <td style="background-color: red;" colspan="2">1</td>
    <td style="background-color: green;" colspan="8">7ffe1e60f9<b>86</b></td>
    <td style="background-color: yellow;" colspan="8">7ffe1e60f9<b>88</b></td>
    <td></td>
    <td>...</td>
  </tr>
</table>
`a` is red here, `b` is green, and `c` is yellow. I am also assuming that `c`
starts at `0x7ffe1e60f996`, which is probably correct but irrelevant right now.
<br/><br/>
So, let's digest this:
<br/>
`a` has a value of `1` and an address of `0x7ffe1e60f986`.
<br/>
`b` has a value of `0x7ffe1e60f986` and an address of `0x7ffe1e60f988`.
<br/>
`c` has a value of `0x7ffe1e60f988`.
<br/><br/>
Which is exactly what we had above but this time with a visual illustration.








<br/><br/>
<br/><br/>

### 8. Changing pointer values - the addresses

In previous examples, I've set the `b` pointer to point to the address of
variable `a`. And never changed the value of the `b` pointer. It pointed to `a`
for as long as it existed.
<br/><br/>
But of course that you can change the pointers, just like any other numbers, you
can add them, subtract them, increment them, and so on.
<br/><br/>
Here's an example:
```cpp
paul@alice:~$ cat a.cpp
#include <iostream>
using namespace ::std;
int main() {
  short int a = 1;
  short int b = 3;
  cout << "a=" << a << endl;
  cout << "b=" << b << endl;

  // c points to a, so *c is 1
  short int * c = &a;
  cout << "c=" << c << endl;
  cout << "*c=" << *c << endl;
  (*c)++;

  // a is now 2 because it was incremented through the c pointer
  cout << "a=" << a << endl;
  cout << "b=" << b << endl;

  // c will point now to b
  c = &b;
  cout << "c=" << c << endl;
  cout << "*c=" << *c << endl;
  (*c)++;

  // b is now 4 because it was incremented through the c pointer
  cout << "a=" << a << endl;
  cout << "b=" << b << endl;

  return 0;
}
paul@alice:~$ g++ a.cpp -o a.exe && ./a.exe
a=1
b=3
c=0x7ffff675532c
*c=1
a=2
b=3
c=0x7ffff675532e
*c=3
a=2
b=4
```
All good, but you might come across the need to make a pointer point to
<b>nothing</b>. Is that even possible?
<br/><br/>
Well, it is possible! For this scenario, when you need to make a pointer point
to "nothing", there is a special value that we give to pointers. The value is
very special, I'm not sure if I'm allowed to share it with you - it's kind of a
secret :) . OK, I'll tell you - the value is zero (`0`).
<br/><br/>
You see, by convention, all un-initialized pointers, or pointers that point to
nothing should be set to `0`. Anyway there is nothing saved at index `0`, in the
first memory byte/cell.
<br/><br/>
The memory address `0` is called a <b>null pointer</b>, and there even is a
keyword / definition for it. In C and C++ it is `NULL` and in C++ it is
`nullptr`.
<br/><br/>
You should also know that dereferencing a null pointer will crash your program.
Here's an example you can try:
```cpp
paul@alice:~$ cat a.cpp
#include <iostream>
using namespace ::std;
int main() {
  short int * c = NULL;
  cout << "c=" << c << endl;
  short int d = *c;
  cout << "The program will not print this because it crashed"
    " at the previous line" << endl;

  return 0;
}
paul@alice:~$ g++ a.cpp -o a.exe && ./a.exe
c=0
Segmentation fault (core dumped)
```
<br/><br/>








<br/><br/>
<br/><br/>

### 9. Reference

A reference can only be created in C++, and it works a lot like a pointer,
except, it's meant to be easier to work with, but it also has a few
disadvantages.
<br/><br/>
So, for any type `X`, the data type `X *` is a pointer to an instance of type X,
and the data type `X &` is a reference to an instance of type X.
<br/><br/>
Working with the reference is easier in a way, because you don't have to
de-reference it, (the `*` operator) like we had to when working with pointers.
<br/><br/>
Let's see a simple example:
```cpp
paul@alice:~$ cat a.cpp
#include <iostream>
using namespace ::std;
int main() {
  int a = 999;
  int * p = &a;
  int & r = a; // this will not be a copy !

  // Let's print the value of a throught the pointer, and through the reference
  cout << "value=" << *p << endl;
  cout << "value=" << r << endl;
  return 0;
}
paul@alice:~$ g++ a.cpp -o a.exe && ./a.exe
value=999
value=999
```

As you can see, we can do the same thing with a pointer or a reference. But,
from a perspective, it's easier to work with references, because when you
initially set it, you can just use `a` instead of `&a`. and when you use it, you
can get it's value using just it's name `r` instead of `*p`. So, in a way, we
are avoiding the use of operators `&` ("address of") and `*`("the value that is
in memory at address").
<br/><br/>
However, the reference also has some disadvantages. For example, you can't reuse
it, you can't make it point to some other variable / value.
```cpp
paul@alice:~$ cat a.cpp
#include <iostream>
using namespace ::std;
int main() {
  int a = 999;
  int b = 888;
  cout << "a=" << a << endl;
  cout << "b=" << b << endl;
  cout << endl;

  // Set pointer and reference, initially to a
  int * p = &a;
  int & r = a;

  // Let's print the value throught the pointer, and through the reference
  cout << "value=" << *p << endl;
  cout << "value=" << r << endl;
  cout << endl;

  // Set pointer and reference, to b ?
  p = &b;
  r = b;

  // Let's print the value throught the pointer, and through the reference
  cout << "value=" << *p << endl;
  cout << "value=" << r << endl;
  cout << endl;

  cout << "a=" << a << endl;
  cout << "b=" << b << endl;
  return 0;
}
paul@alice:~$ g++ a.cpp -o a.exe && ./a.exe
a=999
b=888

value=999
value=999

value=888
value=888

a=888
b=888
```
You can see that something is wrong with this. The value of `a` was `999` at
start, and at the end it's `888`, though we didn't change it ... or did we?
<br/><br/>
Although we didn't mean to change it, when we did `r = b;`, that doesn't change
the reference, in order to make it point to `b`. Instead, our reference `r` is
still pointing (referring) to `a`, so instead of assigning to `r`, we're
actually assigning to `a`. So `r = b;` is, in fact, `a = b;`.
<br/><br/>
Also, with references, you can't link them in order to create a reference to a
reference. But you can do this with pointers.
<br/><br/>
Also, you can't have a reference, refer to nothing. There is no `NULL` as there
is in the case of pointers.
<br/><br/>
I recommend using references wherever is possible, and only switch to pointers
when it's needed. References are easier to understand and you'll find that more
people know how to handle references in code, but a lot of people are confused
when working with pointers. So, if you want your code to be easy to understand
and change by other developers, then you might want to use references where
possible.
<br/><br/>










<br/><br/>
<br/><br/>

### 10. C-style array and sizeof

The C language, allows us to store multiple instances of the same type, in the
same variable.
<br/><br/>
While `int a` will allocate space for one integer(4 bytes), `int b[2]` will
allocate space for 2 integers(8 bytes). In this case, `b` is known to have the
type `int[2]`.
<br/><br/>
You each value is accessible by the index inside this array. Let's see an
example:
```cpp
paul@alice:~$ cat a.cpp
#include <iostream>
using namespace ::std;
int main() {
  int b[2] = { 7, 6 };
  cout << "b[0]=" << b[0] << endl;
  cout << "b[1]=" << b[1] << endl;
  return 0;
}
paul@alice:~$ g++ a.cpp -o a.exe && ./a.exe
b[0]=7
b[1]=6
```
And let's imagine how this looks like in the memory:
<table class="memory_table">
  <tr>
    <td style="width: 180px;">memory byte <b>address</b></td>
    <td>0</td>
    <td>1</td>
    <td>2</td>
    <td>3</td>
    <td>4</td>
    <td>5</td>
    <td>6</td>
    <td>7</td>
    <td>8</td>
    <td>9</td>
    <td>10</td>
    <td>...</td>
  </tr>
  <tr>
    <td style="width: 180px;">memory byte <b>value</b></td>
    <td></td>
    <td></td>
    <td style="background-color: blue;" colspan="4">7</td>
    <td style="background-color: red;" colspan="4">6</td>
    <td></td>
    <td>...</td>
  </tr>
  <tr>
    <td style="width: 180px;"><b>array index</b></td>
    <td></td>
    <td></td>
    <td colspan="4"><b>0</b></td>
    <td colspan="4"><b>1</b></td>
    <td></td>
    <td>...</td>
  </tr>
</table>
<br/>
You should remember that the values(cells) in an array always define a
contiguous memory space. The values are stored in memory one after the other,
without any gaps between them.
<br/><br/>
You should also know that in the previous case, `b` of type `int[2]` can also be
seen as (is a) pointer of type `int *`. In fact, if you print the value of `b`,
you will see a memory address. That is in fact the address of the first integer
value in the array. But remember that in fact the address of a value is the
address of the first byte in that value.
<br/><br/>
So, let's look at this again:
```cpp
paul@alice:~$ cat a.cpp
#include <iostream>
using namespace ::std;
int main() {
  int b[2] = { 7, 6 };
  cout << "b=" << b << endl;
  cout << "b[0]=" << b[0] << endl;
  cout << "address of b[0]=" << &b[0] << endl;
  cout << "b[1]=" << b[1] << endl;
  cout << "address of b[1]=" << &b[1] << endl;
  return 0;
}
paul@alice:~$ g++ a.cpp -o a.exe && ./a.exe
b=0x7ffdf64c3610
b[0]=7
address of b[0]=0x7ffdf64c3610
b[1]=6
address of b[1]=0x7ffdf64c3614
```
<table class="memory_table">
  <tr>
    <td style="width: 180px;">memory byte <b>address</b></td>
    <td>...</td>
    <td style="width: 100px;">7ffdf64c36<b>09</b></td>
    <td style="width: 100px;">7ffdf64c36<b>10</b></td>
    <td style="width: 70px;">...4c36<b>11</b></td>
    <td style="width: 70px;">...4c36<b>12</b></td>
    <td style="width: 70px;">...4c36<b>13</b></td>
    <td style="width: 70px;">...4c36<b>14</b></td>
    <td style="width: 70px;">...4c36<b>15</b></td>
    <td style="width: 70px;">...4c36<b>16</b></td>
    <td style="width: 70px;">...4c36<b>17</b></td>
    <td style="width: 100px;">7ffdf64c36<b>18</b></td>
    <td>...</td>
  </tr>
  <tr>
    <td style="width: 180px;">memory byte <b>value</b></td>
    <td>...</td>
    <td></td>
    <td style="background-color: blue;" colspan="4">7</td>
    <td style="background-color: red;" colspan="4">6</td>
    <td></td>
    <td>...</td>
  </tr>
  <tr>
    <td style="width: 180px;"><b>array index</b></td>
    <td>...</td>
    <td></td>
    <td colspan="4"><b>0</b></td>
    <td colspan="4"><b>1</b></td>
    <td></td>
    <td>...</td>
  </tr>
</table>
<br/>
So, you can see that `&b` has the same value as `&b[0]`. They both point to the
first byte in the first integer.
<br/><br/>
You can also note that `&b[1]` = `&b[0]` + `4`. That `4` is the size of your
data type. This is because `b[1]` is located in memory next to `b[0]`.
<br/><br/>
In general you can know that for a vector named `a`, of type `X`, whatever it's
size is, we have: `a[n] = *(a + n * sizeof(X))`. That is, the memory
address `a` plus, your index(`n`) multiplied by the size of your data type
(`X`).
<br/><br/>
But there is a catch, `a + 4` will be interpreted by the compiler as
`a + 4 * sizeof(X)`. You see, when you are adding to a pointer, by default, the
compiler assumes that you are trying to jump to the next element of the same
type, in an array.
<br/><br/>
Here is an example:
```cpp
paul@alice:~$ cat a.cpp
#include <iostream>
using namespace ::std;
int main() {
  int b[2] = { 7, 6 };
  cout << "b[0]=" << b[0] << endl;
  cout << "b[0]=" << *(b + 0) << endl;
  cout << "b[0]=" << *((int*)((char*)(b) + 0 * sizeof(int))) << endl;
  cout << endl;
  cout << "b[1]=" << b[1] << endl;
  cout << "b[1]=" << *(b + 1) << endl;
  cout << "b[1]=" << *((int*)((char*)(b) + 1 * sizeof(int))) << endl;
  return 0;
}
paul@alice:~$ g++ a.cpp -o a.exe && ./a.exe
b[0]=7
b[0]=7
b[0]=7

b[1]=6
b[1]=6
b[1]=6
```
<br/>
You can note that I had to do some extra casts in order to express to the
compiler my real intention. I am casting `b` from an original data type of
`int *` to a data type of `char *`. I'm doing the math in type `char *` and then
converting it all back to `int *`.
<br/><br/>
Again, if `b` is of type `char *`, then `b + 1` means `b + 1`. But, if `b` is of
type `int *`, then `b + 1` means `b + 4`. That is, `4` bytes, or `1` integer
(`sizeof(int)=4`) to the right in the memory.
<br/><br/>
You should also know that while `sizeof` can be applied to a data type and it
returns the number of bytes required to store a value of that type. `sizeof` can
also be applied to array variables, and it returns the number of bytes required
to store the entire array in memory. That is the number of elements in the array
multiplied by the sizeof of the array type:
```cpp
paul@alice:~$ cat a.cpp
#include <iostream>
using namespace ::std;
int main() {
  int a[0] = {};
  int b[2] = { 7, 6 };
  int c[] = { 1, 2, 3 };
  int d[] = {};
  int e[] = { 45 };
  int f = 54;
  cout << "sizeof(a)=" << sizeof(a) << endl;
  cout << "sizeof(b)=" << sizeof(b) << endl;
  cout << "sizeof(c)=" << sizeof(c) << endl;
  cout << "sizeof(d)=" << sizeof(d) << endl;
  cout << "sizeof(e)=" << sizeof(e) << endl;
  cout << "sizeof(f)=" << sizeof(f) << endl;
  return 0;
}
paul@alice:~$ g++ a.cpp -o a.exe && ./a.exe
sizeof(a)=0
sizeof(b)=8
sizeof(c)=12
sizeof(d)=0
sizeof(e)=4
sizeof(f)=4
```

So, 4, 8 and 12 are actually 1, 2 and 3 elements arrays.
`3 elements * 4 bytes/element = 12 bytes`.
<br/><br/>
So, you can see that you can also initialize an array without specifying it's
size. If you specify the elements in curly braces `{ ... }`, the compiler can
tell how many elements there are. However you can't do `int x[] = NULL`. Because
since you're not giving an array size at declaration time `int x[]`, it will try
to deduce it from the value(initializer list) - but you're not giving a list
there, so, it can't compile, because it doesn't know how much memory it needs to
reserve.
<br/><br/>
If it's not clear enough, the index in the array `X a[N]` goes from `0` to `N-1`
, so you shouldn't access `a[N]` , `a[N+1]` , and so on. They don't exist in the
array, although the memory formula would work, and you could access those bytes:
```cpp
paul@alice:~$ cat a.cpp
#include <iostream>
using namespace ::std;
int main() {
  int b[2] = { 7, 6 };
  cout << "b[1]=" << b[1] << endl;
  cout << "b[2]=" << b[2] << endl;
  cout << "b[22]=" << b[22] << endl;
  return 0;
}
paul@alice:~$ g++ a.cpp -o a.exe && ./a.exe
b[1]=6
b[2]=-802877184
b[22]=779178473
```
You can see that the code compiles and the program runs just fine, although we
are doing something illegal, we're reading from outside the bounds. The values
we are reading are random / garbage values.
<br/><br/>










<br/><br/>
<br/><br/>

### 11. Buffer

Now that you know what an array is, you should know that a buffer is just an
array of 1-byte characters.
```cpp
paul@alice:~$ cat a.cpp
#include <iostream>
using namespace ::std;
int main() {
  char a[255];
  for(unsigned char i=0; i<=254; i++){
    a[i] = i;
  }
  cout << a[65] << a[66] << a[67] << a[68] << endl;
  return 0;
}
paul@alice:~$ g++ a.cpp -o a.exe && ./a.exe
ABCD
```
As you can see, I'm creating a 255 characters array, and each cell contains it's
index as value. So, when we're printing the values at index 65, 66, 67 and 68,
we're printing the characters with that ASCII code, which is `ABCD`.
<br/><br/>
So, any bytes can be stored in an array and treated as a buffer.
<br/><br/>
However, when you are trying to print the contents / characters inside a buffer,
there is a catch:
```cpp
paul@alice:~$ cat a.cpp
#include <iostream>
using namespace ::std;
int main() {
  char a[255];
  a[0]=1;
  for(unsigned char i=1; i<=254; i++){
    a[i] = i;
  }
  cout << "4 chars in a are: \""
    << a[65] << a[66] << a[67] << a[68] << "\"" << endl;
  cout << "whole a is: \"" << a << "\"" << endl;
  return 0;
}
paul@alice:~$ g++ a.cpp -o a.exe && ./a.exe
4 chars in a are: "ABCD"
whole a is: "


123456789:;<=>?@ABCDEFGHIJKLMNOPQRSTUVWXYZ[\]^_`abcdefghijklmnopqrstuvwxyz{|}~"
```
You'll see that the program starts printing characters from the memory, and it
goes on to the next character, it will print that one too, and so on. There is
no stopping rule. The compiler doesn't assume that you want to stop when the
buffer ends(based on how many characters / bytes you allocated). You can have a
buffer that is only partially filled. So for example only 50 characters loaded
into a 100 characters buffer. So, whenever you are printing data from a buffer,
you should tell the compiler the start position(pointer to the first character)
and the end position(pointer to the last character). Or the start position
(pointer to the first character) and the number of characters that you want to
print:
```cpp
paul@alice:~$ cat a.cpp
#include <iostream>
using namespace ::std;
int main() {
  // initialize all characters with value 32 (space character)
  char a[255];
  a[0]=1;
  for(unsigned char i=1; i<=254; i++){
    a[i] = i;
  }
  cout << "4 chars in a are: \""
    << a[65] << a[66] << a[67] << a[68] << "\"" << endl;

  cout << "cout.write(): \"";
  cout.write(a + 65, 4);
  cout << "\"" << endl;
  return 0;
}
paul@alice:~$ g++ a.cpp -o a.exe && ./a.exe
4 chars in a are: "ABCD"
cout.write(): "ABCD"
```
So, `cout.write()` takes 2 arguments: a pointer to the first character which
should be printed, and the number of characters that should be printed. So, in
this case, `a + 65` will be the address of the 65th character in the array.
<br/><br/>










<br/><br/>
<br/><br/>

### 12. C-string
However, if you want to store non binary data, that is simple alpha-numeric text
that contains only usual characters like letters(upper and lowercase, digits,
punctuation), then you can use <b>C-strings</b>. These are arrays of constant
characters, so just pointers to the first character of the string, but the rule
is that the strint ends at the first null character. A null character is the
character whose numerical value is `0`. It is also represented as `'\0'`.
<br/><br/>
For example, a string literal like `"test"` might seem like it requires 4 bytes
of memory to be stored, but in fact, it has an extra null character at the end,
which is the 5th character. So, the following are in fact storing the same
content. <b>Note!</b> that when printing a C-string, the program will print all
the characters until the first null character.
```cpp
paul@alice:~$ cat a.cpp
#include <iostream>
using namespace ::std;
int main() {
  const char a[] = {'t', 'e', 's', 't', '\0'};
  const char * b = "test";
  cout << "a=\"" << a << "\"" << endl;
  cout << "b=\"" << b << "\"" << endl;
  return 0;
}
paul@alice:~$ g++ a.cpp -o a.exe && ./a.exe
a="test"
b="test"
```
So using string literals is easier than using an initializer list to initialize
an array that ends with a null character.
<br/><br/>
To futher support the idea that printing C-strings stops at the first null
character, we can look at the following example, which is an atypical C-string,
because it "contains" a null character inside it, or it contains another string
after it:
```cpp
paul@alice:~$ cat a.cpp
#include <iostream>
using namespace ::std;
int main() {
  const char a[] = {'t', 'e', 's', 't', '\0', 's', 't', 'r', 'i', 'n', 'g', '\0'};
  const char * b = "test\0string";
  cout << "a=\"" << a << "\"" << endl;
  cout << "b=\"" << b << "\"" << endl;
  return 0;
}
paul@alice:~$ g++ a.cpp -o a.exe && ./a.exe
a="test"
b="test"
```
So, we could see this as one string containing a null character or as the string
`"test"` and the string `"string"`. So, as I said, the program sees the string
up to the first null character, and it stops printing it there.
<br/><br/>
However, if you want to print the rest of the string, you can make a new pointer
to the string `"string"`, as an offset of the original string. Or you can treat
this as a buffer, and print it's entire content without stopping at the first
null character.
```cpp
paul@alice:~$ cat a.cpp
#include <iostream>
using namespace ::std;
int main() {
  const char * b = "test\0string";
  const char * c = b + 5; // skip 5 chars, the first string
  cout << "second string=\"" << c << "\"" << endl;
  cout << "b=\"";
  cout.write(b, 11); // 11 characters in the buffer, excluding the final null char
  cout << "\"" << endl;
  return 0;
}
paul@alice:~$ g++ a.cpp -o a.exe && ./a.exe
second string="string"
b="teststring"
```
Note how, when printing the entire buffer, the output is `"teststring"`, which
seems to be missing the null character inside it. But in fact the null character
is there, but it is not printable / printed.
<br/><br/>
Sure, the C language also offers a lot of functions for doing common operations
with C-strings. These functions are declared in the `cstring` header, so, we'll
have to include that header in our cpp. I'll just show some of these here, these
are not all of the available functions.
```cpp
paul@alice:~$ cat a.cpp
#include <cstring>
#include <iostream>
using namespace ::std;
int main() {
  const char * a = "test";
  const char * b = "string";

  cout << "a=\"" << a << "\"" << endl;
  cout << "b=\"" << b << "\"" << endl;

  // Length of a C-string
  cout << "length of b=" << strlen(b) << endl;

  // Concatenate 2 C-strings
  char sum_sizes = strlen(a) + strlen(b) + 1; // +1 for null char at the end
  char c[sum_sizes] = {0}; // initialize all values with zero ("empty string")
  strcat(c, a); // add a to c
  strcat(c, b); // then add b to c (after a)
  cout << "concatenation of a and b is \"" << c << "\"" << endl;

  // Compare 2 C-strings
  int result = strcmp(a, b);
  if(result == 0)
  {
    cout << "\"" << a << "\" is equal with \"" << b << "\"" << endl;
  }
  else if(result < 0)
  {
    cout << "\"" << a << "\" is smaller than \"" << b << "\"" << endl;
  }
  else // if(result > 0)
  {
    cout << "\"" << a << "\" is greater than \"" << b << "\"" << endl;
  }

  // Search for a character in a string(return first occurrence or NULL)
  const char * found_character = strchr(a, 's');
  if(found_character != NULL)
  {
    // I'm converting the char pointer to an integer in order to print it
    // or else, it would print the string starting from the 's' char to
    // the end of the string (null char).
    cout << "The character 's' was found in the string \"" << a << "\""
      " at memory address " << (int*)found_character << endl;
    unsigned short int index_in_string = found_character - a;
    cout << "That is, at index " << index_in_string << " inside the string"
      << endl;
  }
  else
  {
    cout << "The character 's' was not found in the string \"" << a
      << "\"" << endl;
  }

  return 0;
}
paul@alice:~$ g++ a.cpp -o a.exe && ./a.exe
a="test"
b="string"
length of b=6
concatenation of a and b is "teststring"
"test" is greater than "string"
The character 's' was found in the string "test" at memory address 0x55a7d903900a
That is, at index 2 inside the string
```
<br/><br/>










<br/><br/>
<br/><br/>

### 13. CPP ::std::string
C++ comes with a class for working with strings. The class named `string` from
the `std` namespace, which is defined in the `string` header from the C++
standard library (libstdc++) is of great help.
<br/><br/>
Again, this text doesn't try to show everything you can do with the class, but
I'll do the same operations we did earlier with C-strings, but this time using
`::std::string`, just to show a bit of how you can work with it:
```cpp
paul@alice:~$ cat a.cpp
#include <string>
#include <iostream>
using namespace ::std;
int main() {
  string a = "test";
  string b = "string";

  cout << "a=\"" << a << "\"" << endl;
  cout << "b=\"" << b << "\"" << endl;

  // Length of a string
  cout << "length of b=" << b.length() << endl;

  // Concatenate 2 strings
  cout << "concatenation of a and b is \"" << a+b << "\"" << endl;

  // Compare 2 C-strings
  if(a == b)
  {
    cout << "\"" << a << "\" is equal with \"" << b << "\"" << endl;
  }
  else if(a < b)
  {
    cout << "\"" << a << "\" is smaller than \"" << b << "\"" << endl;
  }
  else // if(a > b)
  {
    cout << "\"" << a << "\" is greater than \"" << b << "\"" << endl;
  }

  // Search for a string in a string(return first occurrence or npos)
  unsigned long long int found_character = a.find("s");
  if(found_character != ::std::string::npos)
  {
    cout << "The string \"s\" was found in the string \"" << a << "\""
      " at index " << found_character << endl;
  }
  else
  {
    cout << "The string \"s\" was not found in the string \"" << a
      << "\"" << endl;
  }

  return 0;
}
paul@alice:~$ g++ a.cpp -o a.exe && ./a.exe
a="test"
b="string"
length of b=6
concatenation of a and b is "teststring"
"test" is greater than "string"
The string "s" was found in the string "test" at index 2
```
<br/><br/>
Please remember that a C++ `::std::string` can also be used as a buffer. In
other words, it can be used to hold any byte values, not just printable
characters. So a `::std::string` can be used to store `.mp3` files, or any
binary data.
<br/><br/>
Also remember that you can convert from a C-string to a C++ string using:
```cpp
  const char * a = "test"; // this is a C-string
  const string b = ::std::string(a); // this is a C++ string
```
So, just call the constructor and it can take a C-string and produce a C++
string. To convert it from a C++ string into a C-string, you can use the
`.c_str()` method:
```cpp
  const string b = "some string"; // this is a C++ string
  const char * c = b.c_str(); // this is a C-string
```
<br/><br/>













<br/><br/>
<br/><br/>

### 14. Endianness
As wikipedia says: "<i>In computing, endianness is the order or sequence of
bytes of a word of digital data in computer memory. Endianness is primarily
expressed as <b>big-endian</b> (BE) or <b>little-endian</b> (LE). A big-endian
system stores the most significant byte of a word at the smallest memory address
and the least significant byte at the largest. A little-endian system, in
contrast, stores the least-significant byte at the smallest address.</i>"
<br/><br/>
Let's see how the value `789` is stored in the memory as a `unsigned short int`:
```cpp
paul@alice:~$ cat a.cpp
#include <iostream>
using namespace ::std;

int main()
{
  unsigned short int a = 789;
  const char * p = (const char*)(&a);
  for(unsigned char i=0; i<sizeof(unsigned short int); i++)
  {
    // pointer to each byte of a, and print that byte
    cout << " " << (int)(*(p+i));
  }
  cout << endl;
  return 0;
}
paul@alice:~$ g++ a.cpp -o a.exe && ./a.exe
 21 3
```
As you know, one byte can only hold one of the 256 different values - it can't
hold more than that. So `789` is seen as `3 * 256 + 21`. And as you can see, the
computer stores our `unsigned short int` on `2` bytes, and the value of the
first byte is `21` and the value of the second byte is `3`.
<br/><br/>
So, instead of representing this `unsigned short int` as we used to:
<table class="memory_table">
  <tr>
    <td style="width: 180px;">memory byte <b>address</b></td>
    <td>0</td>
    <td>1</td>
    <td>2</td>
    <td>3</td>
    <td>4</td>
    <td>5</td>
    <td>6</td>
    <td>...</td>
  </tr>
  <tr>
    <td style="width: 180px;">memory byte <b>value</b></td>
    <td></td>
    <td></td>
    <td style="background-color: green;" colspan="2">789</td>
    <td></td>
    <td></td>
    <td></td>
    <td>...</td>
  </tr>
</table>
We can now represent it more accurately as:
<table class="memory_table">
  <tr>
    <td style="width: 180px;">memory byte <b>address</b></td>
    <td>0</td>
    <td>1</td>
    <td>2</td>
    <td>3</td>
    <td>4</td>
    <td>5</td>
    <td>6</td>
    <td>...</td>
  </tr>
  <tr>
    <td style="width: 180px;">memory byte <b>value</b></td>
    <td></td>
    <td></td>
    <td style="background-color: green;">21</td>
    <td style="background-color: green;">3</td>
    <td></td>
    <td></td>
    <td></td>
    <td>...</td>
  </tr>
</table>
<br/><br/>
In general, a numeric value `X` is stored as 1-byte values `a`, `b`, `c`, ...,
where `X = a*256^0 + b*256^1 + c*256^2 + ...`. So `a`, `b`, `c`, ... are each
multiplied by powers of `256` and then summed up.
<br/><br/>
For example `83293 = 93*256^0 + 69*256^1 + 1*256^2` would require 3 bytes to be
stored, the bytes `93`, `69` and `1`. But since we don't have 3-byte integer
data types, and since this can't fit into a 2-bytes integer, we must store it
in a 4-bytes integer. So, for our 4th byte we could consider `+ 0*256^3`, so the
4th byte would be `0`.
<br/><br/>
Comming back to our previous example, `789 = 21*256^0 + 3*256^1` so our byte
values(`a` and `b`) are `21` and `3`.
<br/><br/>
Now let's come back to the endianness. Our most significant byte is `3` because
that byte's value is multiplied with the highest power of 256. In our case byte
`21` is multiplied by 256 to the power of `0`, and byte `3` is multiplied by 256
to the power of `1`. So the highest power of 256 is `1` and the value of that
corresponding byte is `3`.
<br/><br/>
OK, and because our most significant byte is stored at the largest memory
address, we conclude that our program is ran on a little-endian system.
<br/><br/>
All processors must be designated as either big-endian or little-endian. For
example, the 80×86 processors from Intel® and their clones are little-endian,
while Sun's SPARC, Motorola's 68K, and the PowerPC® families are all big-endian.
<br/><br/>
Big-endian and little-endian are only "normal order" and "reverse order" from a
human perspective. Big-endian is indeed easier for humans because it does not
require rearranging the bytes. So, it is a matter of seeing `789` as
`21*256^0 + 3*256^1`, so, as bytes `21` and `3` on a little-endian system or
seeing `789` as `3*256^1 + 21*256^0`, so, as bytes `3` and `21` on a big-endian
system. In other words, little-endian uses increasing powers of 256, and
big-endian uses decreasing powers of 256. As humans, we're used to think
"<i><b>256</b> goes <b>3</b> times into <b>789</b></i>" and after a short math
(<i>3 * 256 = 768</i> and <i>789 - 768 = <b>21</b></i>) we conclude that the
remainder is <b>21</b>. So that's why big endian seems like the "normal order"
of bytes for us, humans. But for other reasons(that I won't get into), some
processors preffer the little endianness.
<br/><br/>













<br/><br/>
<br/><br/>

### 15. Bits in a byte
If you want to go even deeper into how values are kept in memory, you can even
look at the individual bits inside a byte.
<br/><br/>
So, let's go just a little bit into these details, maybe it would be important
at some point to know the bit values.
<br/><br/>
Sure, you can compute the bits of a number using the mathematical approach, I'm
going to skip that, and offer something that's easier to use. There is a
function (template function) that is defined in the `bitset` header. The name of
the function is also `bitset`. The function receives 2 arguments. The first one
is a template argument, which represents the number of digits(bits) that you
want in the output, and the second argument(which is a normal function argument)
is the numeric value that you want to convert to base 2(binary).
<br/><br/>
I will not go into the details of template functions or templates in general.
But here's the code, the function ss pretty simple to use:
```cpp
paul@alice:~$ cat a.cpp
#include <iostream>
#include <bitset>
using namespace ::std;
int main()
{
  unsigned char a = 254;
  cout << ::std::bitset<8>(a) << endl;
  return 0;
}
paul@alice:~$ g++ a.cpp -o a.exe && ./a.exe
11111110
```
So the number `254` in base 10 (decimal) is the number `11111110` in base 2
(binary). Here's a short check:
`0*2^0 + 1*2^1 + 1*2^2 + 1*2^3 + 1*2^4 + 1*2^5 + 1*2^6 + 1*2^7 = 254`.
<br/><br/>
So, now thinking about the endianness above, it kind of feels like the entire
memory / disk is just a, single, big, number, in base `256`, doesn't it :-D? As
if the digits of the number are kept in memory bytes, and the 256 base is
implied.
<br/><br/>
A short remark here. Throughout the history, the number of bits in a byte was
not a constant `8`. There are still hardware machines that use bytes with more
or less than `8` bits. So, you can imagine that this has a big impact on
everything else  .
<br/><br/>














<br/><br/>
<br/><br/>

### 16. Function calls - behind the scenes
As part of this mini-training text, I would like to also describe what happens
on the stack when you call a function.
<br/><br/>
Let's take the following example:
```cpp
paul@alice:~$ cat a.cpp
#include <iostream>
using namespace ::std;

int f(int d, int e)
{
  int g = 0;
  g = d + e;
  return g;
}

int main()
{
  int a = 1;
  int b = 2;
  int c = 0;
  c = f(a, b);
  cout << c << endl;
  return 0;
}
paul@alice:~$ g++ a.cpp -o a.exe && ./a.exe
3
```
It unsurprisingly returns `3` as the sum of `1` and `2`. "Wow" ! But wait, this
isn't about the result, it's about what happens until it reaches to the result.
<br/><br/>
So, we can see that in our program, in the `main` function, we are calling the
`f` function and we are passing the `a` and `b` parameters. Then we store the
return value into `c`. Let's dig deeper into this.
<br/><br/>
1. `main` will allocate space on stack for the return value of the function `f`.
  Since `f` returns an `int`, `main` will allocate `4` bytes. Note that in some
  situations, a function's return value might be passed from the calee to the
  caller through a CPU's register.
2. `main` will next allocate space on the stack for the parameters that should
  be passed to function `f` (`d` and `e`). But it will allocate the space in
  reverse order. So it will allocate the space for `e` and then allocate the
  space for `d`. Since both are of type `int`, it will allocate `8=2*4` bytes.
  Once the memory is allocated in the stack, `main` will copy the values of `a`
  into the space of `d`, and copy the value of `b` into the space of `e`.
3. `main` will save address of the next instruction(line of code) immediately
  after the call instruction to function `f`. This address will be used later,
  when the `f` function returns, to jump the execution, back to this address
  (here), to the caller function, `main`.
4. Jump to function `f`'s first instruction, and continue execution.

(when function `f`'s return is reached):
5. Function `f` will copy the returned value (value of `g`) to the return
  allocated memory on stack - see item 1. above.
6. `f` will free any stack variables defined inside the `f` function(in this
  case, variable `g`).
7. `f` will jump back to the caller/parent function (`main`), using the address
  saved in the item 3. above, while also removing the address from the stack.
8. Function `main` is now back in control, and it's responsible for freeing the
  stack space allocated for parameters `d` and `e`.
9. Function `main` can now resume it's operations/instructions, and use the
  returned value from the stack memory. In this case, to save it to `c`.

<br/><br/>
For more details, see:
1. <a href="https://www.youtube.com/watch?v=Q2sFmqvpBe0" target="_blank">
  https://www.youtube.com/watch?v=Q2sFmqvpBe0</a> and
2. <a href="https://www.youtube.com/watch?v=XbZQ-EonR_I" target="_blank">
  https://www.youtube.com/watch?v=XbZQ-EonR_I</a>
<br/><br/>














<br/><br/>
<br/><br/>

### 17. Stack and heap memories
When an operating system starts executing a program, it will allocate some of
the computer's RAM memory as "<b><i>the stack</i></b>" RAM memory of that
program. The GNU/Linux operating system usually allocates <b>~ 8 MiB</b> of RAM
for every new process / program that it starts. When the process ends it's
execution, the OS reclaims the stack memory, and is free to use it for something
else.
<br/><br/>
You can imagine it something like this:
<table class="memory_table">
  <tr>
    <td style="width: 180px;">memory byte <b>address</b></td>
    <td style="width: 50px;">0</td>
    <td style="width: 50px;">1</td>
    <td style="width: 50px;">2</td>
    <td style="width: 50px;">3</td>
    <td style="width: 50px;">...</td>
    <td style="width: 50px;">a-1</td>
    <td style="width: 50px;">a</td>
    <td style="width: 50px;">a+1</td>
    <td style="width: 50px;">...</td>
    <td style="width: 50px;">a+b-2</td>
    <td style="width: 50px;">a+b-1</td>
    <td style="width: 50px;">a+b</td>
    <td style="width: 50px;">a+b+1</td>
    <td style="width: 50px;">...</td>
    <td style="width: 50px;">n</td>
  </tr>
  <tr>
    <td style="width: 180px;">memory byte <b>value</b></td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td style="background-color: lightgreen;"></td>
    <td style="background-color: lightgreen;"></td>
    <td style="background-color: lightgreen;">...</td>
    <td style="background-color: lightgreen;"></td>
    <td style="background-color: lightgreen;"></td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
</table>
This might look too complicated, but in fact it is simple. The stack (the light
green part of the memory) is just one part of the memory. It's probably not the
first bytes, and probably not the last bytes of the entire memory. It's probably
somewhere in the middle of the memory space(that doesn't mean it has to be at
the exact center, either). In the table above, we assume that the computer has
<b>n</b> bytes of memory, and the stack starts at byte <b>a</b> and the stack is
of size <b>b</b> bytes. So, as I said, <b>b</b> is usually ~8 MiB on GNU/Linux,
and <b>a</b>(the start position of the stack) is random.
<br/><br/>
<b>The heap</b> memory, on the other side, is everything else. The rest of the
memory available to that machine / operating system.
<br/><br/>
So let's picture this again. The stack is colored in light-green, and the heap
is colored in light blue:
<table class="memory_table">
  <tr>
    <td style="width: 180px;">memory byte <b>address</b></td>
    <td style="width: 50px;">0</td>
    <td style="width: 50px;">1</td>
    <td style="width: 50px;">2</td>
    <td style="width: 50px;">3</td>
    <td style="width: 50px;">...</td>
    <td style="width: 50px;">a-1</td>
    <td style="width: 50px;">a</td>
    <td style="width: 50px;">a+1</td>
    <td style="width: 50px;">...</td>
    <td style="width: 50px;">a+b-2</td>
    <td style="width: 50px;">a+b-1</td>
    <td style="width: 50px;">a+b</td>
    <td style="width: 50px;">a+b+1</td>
    <td style="width: 50px;">...</td>
    <td style="width: 50px;">n</td>
  </tr>
  <tr>
    <td style="width: 180px;">memory byte <b>value</b></td>
    <td style="background-color: lightblue;"></td>
    <td style="background-color: lightblue;"></td>
    <td style="background-color: lightblue;"></td>
    <td style="background-color: lightblue;"></td>
    <td style="background-color: lightblue;"></td>
    <td style="background-color: lightblue;"></td>
    <td style="background-color: lightgreen;"></td>
    <td style="background-color: lightgreen;"></td>
    <td style="background-color: lightgreen;">...</td>
    <td style="background-color: lightgreen;"></td>
    <td style="background-color: lightgreen;"></td>
    <td style="background-color: lightblue;"></td>
    <td style="background-color: lightblue;"></td>
    <td style="background-color: lightblue;"></td>
    <td style="background-color: lightblue;"></td>
  </tr>
</table>
<br/><br/>
Now, whenever you are declaring a variable like `char x = 7;`, the memory space
of that variable will be allocated on(assigned into) the stack. So, the value of
that variable will be stored in the stack memory. Here you can see the variable
`x` colored in green:
<table class="memory_table">
  <tr>
    <td style="width: 180px;">memory byte <b>address</b></td>
    <td style="width: 50px;">0</td>
    <td style="width: 50px;">1</td>
    <td style="width: 50px;">2</td>
    <td style="width: 50px;">3</td>
    <td style="width: 50px;">...</td>
    <td style="width: 50px;">a-1</td>
    <td style="width: 50px;">a</td>
    <td style="width: 50px;">a+1</td>
    <td style="width: 50px;">...</td>
    <td style="width: 50px;">a+b-2</td>
    <td style="width: 50px;">a+b-1</td>
    <td style="width: 50px;">a+b</td>
    <td style="width: 50px;">a+b+1</td>
    <td style="width: 50px;">...</td>
    <td style="width: 50px;">n</td>
  </tr>
  <tr>
    <td style="width: 180px;">memory byte <b>value</b></td>
    <td style="background-color: lightblue;"></td>
    <td style="background-color: lightblue;"></td>
    <td style="background-color: lightblue;"></td>
    <td style="background-color: lightblue;"></td>
    <td style="background-color: lightblue;"></td>
    <td style="background-color: lightblue;"></td>
    <td style="background-color: green;">7</td>
    <td style="background-color: lightgreen;"></td>
    <td style="background-color: lightgreen;">...</td>
    <td style="background-color: lightgreen;"></td>
    <td style="background-color: lightgreen;"></td>
    <td style="background-color: lightblue;"></td>
    <td style="background-color: lightblue;"></td>
    <td style="background-color: lightblue;"></td>
    <td style="background-color: lightblue;"></td>
  </tr>
</table>
But, since the stack is relatively small, you can't fit enough data into it. So
whenever you think you might need to hold a lot of data, like a very-very long
string, with a lot of characters(maybe one book of text inside a single string
variable), you can store it on the heap memory.
<br/><br/>
You can choose when you are creating a variable, if you want to store it on the
stack memory, or on the heap memory.
<br/><br/>














<br/><br/>
<br/><br/>

### 18. malloc() and free()
In C, if you want to store a variable on the heap, you must know the exact
number of bytes, required to store that variable / data. Let's say you need <b>s
</b> bytes. Then you can "allocate" / "reserve" / "prepare" those <b>s
<u>contiguous</u></b> bytes by calling the `malloc` function provided by C,
and offering the value <b>s</b> as a parameter to the `malloc` function. The
function will return a generic pointer of type `void *` - it doesn't use a
specific data type, because it doesn't know what you want to hold in that memory
space. But you can later cast it into your prefered data type pointer. This
pointer will point to the <b>first byte</b> out of those <b>s</b> bytes that you
requested. If, when you are calling `malloc`, the operating system is not able
to allocate those <b>s <u>contiguous</u></b> bytes, the malloc function will
return a NULL void pointer.
```cpp
paul@alice:~$ cat a.cpp
#include <iostream>
using namespace ::std;
int main()
{
  void * p = malloc(10);
  if( p == NULL ){
    cout << "Allocation has failed" << endl;
  } else {
    cout << "Allocation was successful" << endl;
  }
  return 0;
}
paul@alice:~$ g++ a.cpp -o a.exe && ./a.exe
Allocation was successful
```
In this program I try to allocate `10` bytes, and then check to see if the
allocation was successful or not. <b>Note!</b> that the pointer variable `p` is
using `8` bytes (as any pointer) on the <u>stack</u> memory. But I have
allocated another space of `10` bytes on the heap memory. I am storing the
address of those 10 bytes from heap, I'm storing that address on the stack. Of
course, if you are trying to allocate more memry than you currently have
available, the allocation will fail. But remember that your program is not the
only program running on the operating system. The OS keeps track of all
allocated memory and shares statistics about the total memory, total allocated
memory(for all programs running) and total free memory. You should also know
that the heap memory can be fragmented. I'll not go too much into this topic,
but you might not be able to allocate X bytes of memory even though the
operating system has much more the X bytes available. This can be, because of
fragmentation - because malloc is only allocating contiguous / continuous bytes.
<br/><br/>
Now let's store a value into a memory space allocated on heap. I will allocate
1 byte on the heap, in order to store a `char` variable. But instead of giving
the value `1` to `malloc`, I'll give it `sizeof(char)` which is `1`, suggesting
(to the other developers reading this code) that I am trying to allocate space
on the heap for an `char` variable.
```cpp
paul@alice:~$ cat a.cpp
#include <iostream>
using namespace ::std;
int main()
{
  char x = 7; // on stack
  void * p = malloc(sizeof(char));
  if( p == NULL ){
    cout << "Allocation has failed" << endl;
  } else {
    cout << "Allocation was successful" << endl;
    char * y = (char *)p; // on heap
    *y = 8;
    cout << "y=" << (int)(*y) << endl;
  }
  return 0;
}
paul@alice:~$ g++ a.cpp -o a.exe && ./a.exe
Allocation was successful
y=8
```
If the allocation was successful, I am converting that void pointer, into an int
pointer, in order to store my int value inside that heap memory space. Please
note that I am not dereferencing the pointer, if the pointer is `NULL`. Doing
that will crash your program.
<br/><br/>
Let's see it in memory, again. This is a continuation of chapter 17, where I
also created a variable on the stack, `x`, which will contrast with our heap
variable `y`(colored in blue):
<table class="memory_table">
  <tr>
    <td style="width: 180px;">memory byte <b>address</b></td>
    <td style="width: 50px;">0</td>
    <td style="width: 50px;">1</td>
    <td style="width: 50px;">2</td>
    <td style="width: 50px;">3</td>
    <td style="width: 50px;">...</td>
    <td style="width: 50px;">a-1</td>
    <td style="width: 50px;">a</td>
    <td style="width: 50px;">a+1</td>
    <td style="width: 50px;">...</td>
    <td style="width: 50px;">a+b-2</td>
    <td style="width: 50px;">a+b-1</td>
    <td style="width: 50px;">a+b</td>
    <td style="width: 50px;">a+b+1</td>
    <td style="width: 50px;">...</td>
    <td style="width: 50px;">n</td>
  </tr>
  <tr>
    <td style="width: 180px;">memory byte <b>value</b></td>
    <td style="background-color: lightblue;"></td>
    <td style="background-color: lightblue;"></td>
    <td style="background-color: blue;">8</td>
    <td style="background-color: lightblue;"></td>
    <td style="background-color: lightblue;"></td>
    <td style="background-color: lightblue;"></td>
    <td style="background-color: green;">7</td>
    <td style="background-color: lightgreen;"></td>
    <td style="background-color: lightgreen;">...</td>
    <td style="background-color: lightgreen;"></td>
    <td style="background-color: lightgreen;"></td>
    <td style="background-color: lightblue;"></td>
    <td style="background-color: lightblue;"></td>
    <td style="background-color: lightblue;"></td>
    <td style="background-color: lightblue;"></td>
  </tr>
</table>
<br/><br/>
Everything is fine, except, this program has a "<b>memory leak</b>". A memory
leak is a memory space that was allocated on heap, and was not freed until the
end of the program. So, you are asking for memory, you tell the operating system
that you need memory to store your data, and it gives you the memory, but then
you should give it back when you no longer need it. That is <i>at least</i> a
good practice. If you are not giving that memory back to the operating system,
the OS will take it back, anyway, when your program ends execution. For this
reason, some people don't care about memory leaks, or memory leaks go unnoticed.
You will not get an error from your program and you'll not get an error from the
OS, that you forgot to free the allocated memory. However, there are tools that
can help you track memory allocations and report any leaks that your program
might have. Again, I'll not go deeper into mem leak detection tools. But, I will
get back to say that it is <i><b>at least</b></i> a good practice to free your
memory. If you don't you can get into trouble. If you are developing an
application that will not end it's execution anytime soon (like a server that
will be running for months or even years in a row), then you can't afford to
have any memory leaks. That is because that memory will remain allocated / used
and if you repeat the process and keep allocating memory and you never free it,
then, it's only natural that the machine will run out of memory, and your
program will not be able to allocate more memory and might start misbehaving, or
worst, the OS might even kill(abruptly end the execution of) your program.
<br/><br/>
But don't worry, your program is safe, as long as you call the `free` function,
and give it the pointer that you received from `malloc()`, once you no longer
need that heap memory space. So, let's fix our program:
```cpp
paul@alice:~$ cat a.cpp
#include <iostream>
using namespace ::std;
int main()
{
  char x = 7; // on stack
  char * y = (char *)malloc(sizeof(char)); // on heap
  if( y == NULL ){
    cout << "Allocation has failed" << endl;
  } else {
    cout << "Allocation was successful" << endl;
    *y = 8;
    cout << "y=" << (int)(*y) << endl;
    free(y); // avoid memory leak !
  }
  return 0;
}
paul@alice:~$ g++ a.cpp -o a.exe && ./a.exe
Allocation was successful
y=8
```
Note that I casted the result of `malloc` and saved it directly in a `char *`,
thus, avoiding / hiding the `void *`. This is OK, I can cast the pointer even if
it is NULL. I only have to pay attention when dereferencing the pointer, because
(again): "<i>Dereferencing a NULL pointer will crash my application<i>" :-) .
Also note that `free` returns `void`, so it returns nothing back. It should
never fail.
<br/><br/>
Also, very important, note that after `free`-ing the `y` pointer, the `y`
pointer is still holding the memory address of the memory space that was
previously allocated. So, nothing is preventing me from reading / writing to
that memory space. Again, I should not do that. If I freed the memory I should
not use it anymore - I should not read from it, I should not write to it. If I
still need to use it, then I should not free it. However, printing the address
inside pointer `y` wil not hurt, is not illegal. If, despite all these warnings,
I am writing to the memory behind `y`, after freeing it, I should expect
"undefined behavior". That means that the program might crash, it might also not
crash - it depends on other factors, and we can consider the effect random. So,
it's definitely something you would not do in production / if you want your
program to have a predictable outcome.
<br/><br/>
"Undefined behavior" is worst than crashing your program. If your program is
always crashing, at least you can study/investigate the problem and come up with
a fix. But if other/random factors are involved and sometimes your program is
crashing and other times it is not crashing(beyond your control) - then that is
harder to study and fix. So, it's a little bit better if your program is always
crashing.
<br/><br/>
With that in mind, we can avoid the "undefined behavior" by setting a pointer to
`NULL` after `free`-ing it. The use of that memory afterwards (by dereferencing
the pointer), will definitely crash your program. So, doing that is a good
practice too:
```cpp
paul@alice:~$ cat a.cpp
#include <iostream>
using namespace ::std;
int main()
{
  char x = 7; // on stack
  char * y = (char *)malloc(sizeof(char)); // on heap
  if( y == NULL ){
    cout << "Allocation has failed" << endl;
  } else {
    cout << "Allocation was successful" << endl;
    *y = 8;
    cout << "y=" << (int)(*y) << endl;
    free(y); // avoid memory leak !
    y = NULL; // avoid further use
  }
  return 0;
}
paul@alice:~$ g++ a.cpp -o a.exe && ./a.exe
Allocation was successful
y=8
```
<br/><br/>
Also note that the address returned by malloc is "random", in the sense that
your program can not predict where the operating system will reserve that memory
space. But of course that the OS is probably reserving the space in a
predictable way, not randomly. However, you should know that allocating space on
the heap takes some time, it is not instant. The OS needs some time to search
for a contiguous space, big enough to fit your data. So if performance/execution
speed is important for your program, and you can't avoid allocating on the heap,
then you might want to minimize the number of calls to `malloc`.
<br/><br/>
Another note:
```cpp
paul@alice:~$ cat a.cpp
#include <iostream>
using namespace ::std;
int main()
{
  int * y = NULL;
  for(int i=0; i<20; i++){
    y = (int *)malloc(sizeof(int));
    if(y != NULL){
      *y = i;
    }
  }
  if(y != NULL){
    cout << "Last value=" << *y << endl;
  }else{
    cout << "Last value=NULL" << endl;
  }
  return 0;
}
paul@alice:~$ g++ a.cpp -o a.exe && ./a.exe
Last value=19
```
This is an example of bad program design! My program is leaking memory and there
is no "easy" way to fix it. So I am allocating 20 `int`s and assigning a number
0-19 to each one. Those are 20 `malloc` function calls, 20 different memory
spaces, they might not even be next to eachother in memory. Each malloc is
returning 4 bytes of contiguous memory space, 4 consecutive bytes. But those 20
blocks of memory might be randomly spread across the heap memory. And every call
to malloc is replacing the old pointer, thus losing that address before
`free`-ing it. So, if I am attempting to fix this bug by free-ing the pointer
at the end of the for loop, I am breaking the final `cout` which is trying to
print the value inside the last allocated space. So, you should definitely think
about memory allocation and deallocation when you are designing a program,
otherwise you'll get into trouble :-) . I'm not giving you a solution to this
problem. Food for thought :-) .
<br/><br/>
Another note:
```cpp
paul@alice:~$ cat a.cpp
#include <iostream>
using namespace ::std;
int main()
{
  int * y = NULL;
  y = (int *)malloc(9999999999999);
  // allocation will fail - required space too big
  if(y == NULL){
    free(y);
    cout << "OK" << endl;
  }
  y = (int *)malloc(99);
  if(y != NULL){
    free(y); // OK
    cout << "freed the memory 1st time" << endl;
    free(y); // not OK !
    cout << "never printing" << endl;
  }
  return 0;
}
paul@alice:~$ g++ a.cpp -o a.exe && ./a.exe
OK
freed the memory 1st time
free(): double free detected in tcache 2
Aborted (core dumped)
```
You can't free the same pointer a 2nd time - the program will crash.
<br/><br/>














<br/><br/>
<br/><br/>

### 19. new and delete
In C++ the `malloc` and `free` that we talked about are also working, but there
is also the benefit of `new` and `delete`.
<br/><br/>
`new` is doing almost the same thing as `malloc`, but it detects the data-type
that you trying to construct and knows how many bytes to allocate, and if the
allocation fails, it throws an exception, instead of returning `nullptr`. `new`
will also call the constructor if it's creating an object(a class data type).
<br/><br/>
`delete` is doing almost the same thing as `free`, but it also calls the
destructor if it is dealing with a class data type.
<br/><br/>
Here are short examples:
```cpp
paul@alice:~$ cat a.cpp
#include <iostream>
using namespace ::std;
class Dog{
public:
  int legs=4;
  Dog(){ cout << "Dog created" << endl; }
  ~Dog(){ cout << "Dog destroyed" << endl; }
};
int main()
{
  int * a = new int(7);
  cout << "a=" << *a << endl;
  delete a;

  Dog * b = new Dog();
  cout << "Dog b legs=" << b->legs << endl;
  delete b;

  Dog c;
  c.legs -= 1;
  cout << "Dog c legs=" << c.legs << endl;
  // c is on stack, will free automatically

  Dog * d = (Dog *)malloc(sizeof(Dog));
  if (d != nullptr){
    cout << "Dog d legs=" << d->legs << endl;
  }
  free(d);

  Dog * e = new Dog[2]();
  e[0].legs = 0;
  e[1].legs = 1;
  delete[] e;

  // c is on stack, will free now
  return 0;
}
paul@alice:~$ g++ a.cpp -o a.exe && ./a.exe
a=7
Dog created
Dog b legs=4
Dog destroyed
Dog created
Dog c legs=3
Dog d legs=1588645207
Dog created
Dog created
Dog destroyed
Dog destroyed
Dog destroyed
```
You can see that `a` is an `int` on heap, allocated using `new`. `b` is a Dog on
heap. Note how the Dog constructor and destructor is called for `b` (before and
after printing the number of legs - 4). Note how `c` is a Dog on stack, and will
be freed when we exit the `main` scope. `d` is a Dog that was allocated with
`malloc`, but it memory space was not initialized by calling the constructor, so
the number of legs contains a random value that was in memory (`1588645207`).
`e` is actually an array of 2 `Dog`s. These 2 dogs are next to each other in the
heap memory, and they are constructed and destroyed at once (as the whole
array) - that's why you see 2 Dogs being created, and then 2 being destroyed.
Finally, the last "destroy" message comes from `c` which is freed automatically
as we exit the scope.
<br/><br/>
<br/><br/>
Note that while `string s = "My string"` seems to be on stack - the string
contents, the characters, the long text that you can store in it - is actually
in the heap. The string object is on stack, but behind the scenes, inside the
`string` class, it allocates memory in the heap and it stores there the data. So
, things might not be what they seem :-) .
<br/><br/>


















<br/><br/>
<br/><br/>
### 20. Other
Funny things with HEAP memory - you should try creating these scenarios in short
programs to test these theories:
- void* malloc (size_t size) and void free (void* ptr);
  - do not free a reserved memory - mem leak
  - free on stack - Invalid pointer crash
  - call free two times on same mem - double free or corruption
  - call free on a random/hardcoded address - Segmentation fault
  - write to deleted memory - silent corruption
  - write to random/hardcoded address - Segmentation fault
  - free memory allocated with new
  - delete/free on NULL, is ok, it will check and do nothing
- setting a pointer to NULL

