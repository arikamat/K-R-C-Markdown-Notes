---
title: Selected Topics from Chapter 1
created: '2023-08-12T22:47:38.149Z'
modified: '2023-08-12T23:37:57.550Z'
---

# Selected Topics from Chapter 1
## 1.4 Symbolic Constants
A ``#define`` line defines a symbolic name or symbolic constant to be a particular string of characters
```C
# define name replacement list
```
- Any ocurrence of name will be repalced by the correspoinding replacement text. 
- The name has the same form as a variable name: a sequence of letters and digits that begin with a letter. 
- Replacement text can be sany sequence of chars, need not just be numbers
- Conventionally written in uppercase
- No semicolon at the end of a ``#define`` line


## 1.6 Arrays

Fixed Size arrays need to be declared like the following 
```c
int ndigit[10];
```

## 1.8 Arguments - Call by Value

- In C, all function arguments are pass "by value". This means that called function is given values of its arguments in ttemporary variables rather than the originals.

- It is possible to modify external variables by providing the address of a variable

## 1.10 External Variables and Scope

Each local variable in a function comes into existwence only when the function is called, and disappearsj when the funtion is exited. These variables are usually known as **automatic** variables

Automatic varibales do not retain values from one call to the next, and must be explicitly set upon each entry. If not set, they will contain garbage

Alternative: define varable that are external to all functions. These are variables that can be accessed by name by any fucntion. Thehse variables are globally accessible and remain in existence permantly.

An external variable must be **defined**, exactly once, outside of any function - sets aside storage for it

variable must also be **declared** in each function that wants to access it. The declaration may be an explicit ``extern`` statement or implicit from context

Example:
```c
#include <stdio.h>

#define MAXLINE 1000 /* maximum input line size */
int max; /* maximum length seen so far */
char line[MAXLINE]; /* current input line */
char longest[MAXLINE]; /* longest line saved here */
int getline(void);
void copy(void);
/* print longest input line; specialized version */
main() {
    int len;
    extern int max;
    extern char longest[];
    max = 0;
    while ((len = getline()) > 0)
        if (len > max) {
            max = len;
            copy();
        }
    if (max > 0) /* there was a line */
        printf("%s", longest);
    return 0;
}
/* getline: specialized version */
int getline(void) {
    int c, i;
    extern char line[];
    for (i = 0; i < MAXLINE - 1 &&
        (c = getchar)) != EOF && c != '\n';
    ++i)
line[i] = c;
if (c == '\n') {
    line[i] = c;
    ++i;
}
line[i] = '\0';
return i;
}
/* copy: specialized version */
void copy(void) {
    int i;
    extern char line[], longest[];
    i = 0;
    while ((longest[i] = line[i]) != '\0')
        ++i;
}
```

In certain circumstances, ``extern`` declaration can be omitted. If definition of external variable occurs in sourcefile before its use in a particular function, there is no need for ``extern`` declaration.

Typical practice is to put extern declaration of variables and functions in sepearte file, called a header. It's included in source files by the ``#include`` syntax

