<!--
.. title: To GC or not to GC
.. slug: garbage-collector-then-a-boon-now-a-curse
.. date: 2024-05-18 18:14:09 UTC+05:30
.. tags: programming
.. category: programming
.. link: 
.. description: Garbage Collectors solved crucial memory management issues by preventing leaks, but they add runtime overhead, reducing overall performance. With languages like Rust that omits GC for more efficient memory management, it's worth questioning whether new programming languages still need garbage collectors.
.. type: text
-->

**Disclaimer**: this article is meant for beginners and tech enthusiasts and include purely my opinions.

###`Programs require memory management`

Computer memory is a finite resource. By memory, I am referring to the primary memory (RAM) used by the CPU to store, retrieve, and process data of programs during runtime. Typically, an average PC user will have 8 to 16 gigabytes of RAM, which is shared by all the apps running on their PC. Because we have a finite resource in terms of memory, this poses a significant challenge for programmers to effectively manage the resources, i.e., memory, without compromising the reliability and usability of the OS.

We can understand the problem of memory management with the help of an example: Let's suppose you write a program (in any programming language of your choice) that runs indefinitely to collect a bunch of numbers from the user, store them, and print them in your terminal. It would look like this to the user:

```
$ Enter number: 2
[OUT]: [2]
$ Enter number: 4
[OUT]: [2,4]
$ Enter number: 8
[OUT]: [2,4,8]
$ Enter number: 10
[OUT]: [2,4,8,10]
$ Enter number: 6
[OUT]: [2,4,8,10,6]
```

To realize this program I'll write some code in my hypothetical programming language like this:

```
number_store = NULL
while (true) {
    print("Enter number: ");
    x = read_number();
    if (number_store == NULL) {
        number_store = new Integer[1];
    } else {
        number_store_new = new Integer[number_store.current_size + 1];
        copy(number_store, number_store_new);
        number_store_new[number_store.current_size] = x;
        number_store = number_store_new;
    }
    print(number_store);
}
```

The above code is quite straightforward. Here, we allow the user to enter a number and then save that number in an array after increasing its size by one. When we create an array with the new keyword, in the background, the language reserves some memory space. Initially, memory space equivalent to size 1 is reserved, but as the user enters more numbers, more memory is reserved. However, this leads to a fundamental problem! When we create a new array and assign the old variable to this new array, what happens to the old array and the memory we allocated to it during runtime? Where does that memory go?

The answer is that it doesn't go anywhere! It still resides in the memory of our PC. As long as our program executes, the memory we reserved will be locked and unusable by any other program. This phenomenon is also known as a [Memory Leak](https://en.wikipedia.org/wiki/Memory_leak) in computer science lingo.

We can solve this problem by introducing functionality to free up the memory that is no longer requires as follows:

```
number_store = NULL
while (true) {
    print("Enter number: ");
    x = read_number();
    if (number_store == NULL) {
        number_store = new Integer[1];
        number_store[0] = x;
    } else {
        number_store_new = new Integer[number_store.current_size + 1];
        copy(number_store, number_store_new);
        number_store_new[number_store.current_size] = x;
        delete number_store; // deallocate old memory
        number_store = number_store_new; // assign new memory to old variable
    }
    print(number_store);
}
```

However ingenious this solution may look, it's not new. Primitive but high-level programming languages like C and C++ already have library functions such as `alloc`, `free`, `new`, `delete`, etc., that perform this task. But manual memory management like this puts extra strain on developers to write memory management logic in addition to complex business code. Hence, there is a need for a tool or application that can run behind the scenes to manage memory with minimal to no programmer intervention.

###`Garbage Collectors solves memory management`

To solve the problem of automatic garbage collection, the [LISP programming language](https://en.wikipedia.org/wiki/Lisp_(programming_language)) introduced [garbage collection](https://en.wikipedia.org/wiki/Garbage_collection_(computer_science)) for the first time. Garbage collectors are programs that run implicitly during the runtime of a program and free up memory that is no longer required by the program, allowing other programs to utilize the blocks. For example, in our program above, a garbage collector would typically free up the memory space acquired by the old array once we copy the values from the old memory. Garbage collectors can run many algorithms to detect redundant memory space acquired by the program to free them. We won't be going into any such algorithms in this article.

###`Garbage Collectors become problem themselves`

Garbage collectors solved crucial memory management issues by preventing leaks, but they add runtime overhead, reducing overall performance. In the [Java programming language](https://en.wikipedia.org/wiki/Java_(programming_language)), each JVM instance runs its own garbage collector, which makes Java programs inefficient in terms of performance. Running GC in Java causes an increase in pause time while cleaning the memory, overhead in the heap memory, which again resides in the primary memory, increased latency, and more CPU usage. In addition to this, the efficiency of GCs also comes with the underlying running algorithms, which may cause complexity in certain use cases.

###`Can we do without garbage collectors?`

The use of GC in programming languages is a boon for developers, but they do come at a cost. So a natural question arises: can we do without a GC? Or can we figure out a solution where the tradeoff between manually managing the memory and automatically preventing memory leaks can be effective? The answer to this question may lie in the new programming language on the horizon: [Rust](https://en.wikipedia.org/wiki/Rust_(programming_language)).

Rust is a fairly new programming language with C-like syntax that has removed both manual memory management modules as well as the Garbage Collector. Rust has implemented strict rules in syntax to prevent developers from ever occupying redundant memory space by implementing [RAII (Resource Acquisition Is Initialization)](https://en.wikipedia.org/wiki/Resource_acquisition_is_initialization) through proposing the concept of [ownership](https://doc.rust-lang.org/book/ch04-01-what-is-ownership.html). In a nutshell, the concept of ownership tells us that each memory space currently occupied in the language will have an owner. If the owner of the memory dies, is removed, or is reassigned to another memory space, then that memory space is immediately deallocated. Additionally, an owner may own more than one memory space, which is completely fine, but if the owner is removed, then all the memory is also removed.

Implementing solutions like RAII again comes at a cost. Since the onus is now on the language to prevent developers from creating duplicate owners of memory space, it restricts developers from free-style and multi-paradigm coding.

###`Conclusion`

With this, we have reached the end of our article: "To GC or not To GC." Rust has provided us with an example of how a language can operate without the implementation of GC in the language runtime, making itself fast and efficient. The trade-off on syntax strictness is also fair, as developers don't have to worry about manual memory management. However, GCs exist in almost all popular programming languages like Go, Python, Swift, etc., and aren't going anywhere. I believe that in the future, a solution must emerge that tries to solve the problem of memory management with a mix of both concepts: Garbage Collection and RAII.