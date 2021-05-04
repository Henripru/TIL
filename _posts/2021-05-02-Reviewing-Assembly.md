---
layout: post
title: Reviewing Assembly
published: true
tags: c++ x86_x64 assembly
---

### Intro
C++ Weekly With Jason Turner is one of my favorite YouTube channels. Jason always goes to a tool called [Compiler Explorer](https://godbolt.org/) to illustrate features of c++. Having been motivated to better follow his explanations, I decided to refresh my memory by carefully walking through some code. 

### Discussion
I pulled up compiler explorer and started working.

I had this as my c++ code: 

```cpp
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

```nasm
 foo():
         push    rbp
         mov     rbp, rsp
         mov     eax, 10
         pop     rbp
         ret
 main:
        push    rbp
        mov     rbp, rsp
        sub     rsp, 16
        mov     DWORD PTR [rbp-16], 0
        lea     rax, [rbp-16]
        mov     QWORD PTR [rbp-8], rax
        call    foo()
        mov     DWORD PTR [rbp-12], eax
        mov     eax, 0
        leave
        ret
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
- Finally, Lines 15-18 sets the last `z` variable and leaves main procedure.

### Final Thoughts

When I was going through this assembly, I didn't remember how a stack was organized. The analogy I have in my head now is a stack is organized like the floors in a building. Addresses and floors go from the highest number at the top to the lowest number at the bottom. The way stack frames are allocated is the same way as code is executed. From top to bottom.

Finally, a standing question I have is: Why is the address of `y` higher than the address of `z`? If the stack is allocated top down and then frames are filled bottom up, I would expect that `z` would have the highest address, but this isn't the case in this code.
