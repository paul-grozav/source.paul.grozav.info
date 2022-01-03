---
layout: page
ptitle: Pointers and memory
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
  long long int = 9;
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
To be continued ...

