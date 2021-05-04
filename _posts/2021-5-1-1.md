--------
layout: post
title: Reviewing Assembly
published: true
tags: c++ x86_x64 assembly
--------

Jason Turner is one of my favorite YouTubers. He always goes to a tool called [Compiler Explorer](https://godbolt.org/) to illustrate features of c++. Having been motivated to know more assembly, I decided to refresh my memory with some basics. 

I had this as my c++ code: 

```
int foo()
{
    return 10;
}

int main()
{
    int x = 0;
    int* y = &x;
    int z = foo();

    return 0;
}
```

and this was the generated assembly:

```
1  foo():
2          push    rbp
3          mov     rbp, rsp
4          mov     eax, 10
5          pop     rbp
6          ret
7  main:
8          push    rbp
9          mov     rbp, rsp
10         sub     rsp, 16
11         mov     DWORD PTR [rbp-16], 0
12         lea     rax, [rbp-16]
13         mov     QWORD PTR [rbp-8], rax
14         call    foo()
15         mov     DWORD PTR [rbp-12], eax
16         mov     eax, 0
17         leave
18         ret
```

Stepping through, I found the code somewhat familiar.

Starting from the `main:` label: 
- Lines 8-10 are a standard sequence of calls which create and allocate space for the current stack frame. Another name for the process is called an enter 
  - Notice that line 10 subtracts 16 from the value of the stack pointer in order to fit 2x DWORD/(4 byte) integers + 1x QWORD(8 byte) pointer. Originally, I was confused why the pointer would need to be a QWORD, but having done some internet searches, I learned that the size of a pointer is dependent on the architecture of the system you are compiling on. This makes sense because on a 64-bit system, there are 64-bit addresses to deal with.
- Line 11 stores a value of 0 for x.
- Line 12-13 store the address of x in y. 
  - I didn't know how `lea` was working. `lea` is short for Load Effective Address. It stores an address in a register. The expression `[rbp - 16]` was confusing me though. Normally brackets indicate dereferencing an address to copy a value. But with a little searching I found out that [lea has special behavior](https://stackoverflow.com/a/25824111)
- Line 14 jumps to the foo procedure
- Lines 1-6 enters, loads 10 into `eax` and leaves
- Finally, Lines 15-18 set the last z variable and leave main procedure.

When I was going through this assembly, I didn't remember how a stack was organized. The analogy I have in my head now is a stack is organized like the floors in a building. Addresses and floors go from the highest number at the top to the lowest number at the bottom. The way stack frames are allocated is the same way as code is executed. From top to bottom.

Finally, a standing question I have is: Why is the address of `y` higher than the address of `z`? If the stack is allocated top down and then frames are filled bottom up, I would expect that `z` would have the highest address, but this isn't the case in this code.
