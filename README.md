# Heap And Exceptions
This repository contains a single library that manages the memory heap (and therefore allocation), garbage collection, and exceptions for C programs. This is a "science project" that I am doing for fun that supports my other fun projects. I do not recommend that anyone use it in a "production" environment. That having been said, I have been doing this kind of thing for a while and it should just work. :) 

This makes heavy use of the C preprocessor to do things that stretch its intended purpose. If you don't like the preprocessor, then run away. Now. This also uses longjmp to implemente the exceptions. Many people hate that as well. I do not feel that it's justified but here we are. 

This uses its own heap manager and only uses ``malloc()`` and ``free()`` to manipulate the size of the memory available to the program. This allows for greater introspection and trouble shooting. Yes, I know about the various heap debuggers, valgrind, and lint. This does not replace them.

## Heap Management
This library implements its own ``malloc()`` and friends. That does not mean that you cannot use the system ``malloc()`` and friends. But obviously the GC will not work for memory that you allocate that way. This library requires that you put function calls into every function that allocates or frees memory. It is a basic stack-based deterministic GC that frees memory when the function that allocated it returns. Unless, of course a pointer is returned. If that happens then the pointer is pushed on the stack when the function returns. If you assign a pointer to a global variable, that pointer will be destroyed when the function returns. **``NEVER USE GLOBAL VARIABLES``** with the heap library unless you use the correct allocation function.

## Exceptions
Since it is required to have something at the beginning of every function, creating a call stack that can be printed out is possible. A ``CATCH`` block requires a unique number to be defined for it. The ``TRY`` block is aware of those numbers and the call stack maintains a stack of ``setjmp`` data structures to try when an exception is raised. If the exception is "handled" then code continues where the exception was raised. If it's not handled then the program aborts and a call stack is printed. 
