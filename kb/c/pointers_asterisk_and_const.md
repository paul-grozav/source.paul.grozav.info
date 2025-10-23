---
layout: page
title: Pointers - asterisk and const
---

`int* p;` versus `int *p;`

I’ll simply quote Bjarne Stroustrup here:

### Is `int* p;` right, or is `int *p;` right ?

Both are “right” in the sense that both are valid C and C++ and both have exactly the same meaning. As far as the language definitions and the compilers are concerned we could just as well say “int*p;” or “int * p;”

The choice between “int* p;” and “int *p;” is not about right and wrong, but about style and emphasis. C emphasized expressions; declarations were often considered little more than a necessary evil. C++, on the other hand, has a heavy emphasis on types.

A “typical C programmer” writes “int *p;” and explains it “*p is what is the int” emphasizing syntax, and may point to the C (and C++) declaration grammar to argue for the correctness of the style. Indeed, the * binds to the name p in the grammar.

A “typical C++ programmer” writes “int* p;” and explains it “p is a pointer to an int” emphasizing type. Indeed the type of p is int*. I clearly prefer that emphasis and see it as important for using the more advanced parts of C++ well.

The critical confusion comes (only) when people try to declare several pointers with a single declaration:

```cpp
  int* p, p1;	// probable error: p1 is not an int*
```
Placing the * closer to the name does not make this kind of error significantly less likely.

```cpp
  int *p, p1;	// probable error?
```
Declaring one name per declaration minimizes the problem – in particular when we initialize the variables. People are far less likely to write:

```cpp
  int* p = &i;
  int p1 = p;	// error: int initialized by int*
```
And if they do, the compiler will complain.

Quote from Bjarne Stroustrup’s web page: http://www.stroustrup.com/bs_faq2.html#whitespace.

### Constant pointers
```cpp
// 1. pointer to constant character
// a is a pointer (a memory address/location) to a constant character. You can’t
// change that character, but you can set the a pointer to point to another
// location.
const char * a;

// 2. pointer to constant character (same as above)
// b is the same as a.
char const * b;

// 3. constant pointer to character
// c is a constant pointer to a non-constant character. That means that you can
// change the value of the character c points to, but you can’t set c to point
// to another character(memory location).
char * const c;

// 4. constant pointer to constant character
// d is a constant pointer to a constant character. That means that you can’t
// change the value of the pointer(you can’t set the pointer to point somewhere
// else) AND you can’t change the value of the character it points to.
const char * const d;

// 5. constant pointer to constant character (same as above)
// e is the same as d.
char const * const e;
```
