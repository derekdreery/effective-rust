# The C Programming language

Invented by Kernigan and Richtie at Bell Labs in the 60s, C has become the
de-facto standard programming language for low-level systems programming. The
reasons for this are its simplicity and power. In this section I don't want to
write a full C language tutorial, but to point out some of its key features,
and key pitfalls.

C is an expression-based imperative language. That means a program is a list of
instructions, similar to machine code. These instructions are higher-level that
machine code, meaning that one instruction may be equivalent to a number of
machine instructions, and C instructions map more clearly onto real world
problems (an example would be the `8 * 10` expression we saw earlier).

C has the concept of functions, that is re-usable groups of statements to
perform some task. Here is an example of a C function:

```c
int add_int(int x, int y) {
    return x + y;
}
```

The function would then be useable in another code block like so

```c
int x = 2;
int y = 3;

int z = add_int(x, y); /* z now holds the value 5 */
```

Internally this corresponds to jump commands: when a function is called, the
call site location and local variables are stored, then control jumps to the
function location. When a return is encountered (or end of the function), the
processor loads back the call site and local variables, and jumps back.

Higher level languages also introduce *control-flow*. This is constructs that
correspond to jumps, but add constraints to make it easier for users to reason
about their code, and reduce restraints. For example,

```c
int x = 2

if (x) {
    printf("Value is non-zero");
} eles {
    printf("Value is zero");
}
```

This corresponds to something like

```asm
start:
    mov eax,2
    cmp eax,eax
    jez true
false:
    ; write something to i/o
    jmp end
true:
    ; write something to i/o
end:
```

You can actually recreate this in C using the `goto` keyword, but this is
discouraged, and for good reason! It is very easy to make mistakes using jumps
that will be caught at compile time using control-flow statements like `if`.

The final key feature of C I want to talk about is *pointers*. Pointers allow
variables to hold references to other variables, allowing programmers to build
more complex data layouts. It's important to note that on its own, a pointer
value means nothing. If you have the following code

```c
int x = 2;
int* y = &x; /* `int*` is a pointer to an integer, `&x` is the address of x */
```

then the value of `y` viewed as an integer will be some random number like
`0x1F2743AACA5436BF`. This number only has meaning in the context of a block of
memory, when it corresponds to a location in that memory, also known as an
*address*.

Pointers allow us to create complex data structions. Here is an example

```c
/* these functions implement a linked list */

/* We haven't mentioned struct, but it's just a bunch of data types together,
nothing magic */
typedef struct _linked_list_t {
    /* a pointer to the data, we use a pointer so the data can be any size */
    void* data,
    /* a pointer to the next item in the list */
    linked_list* next
} linked_list_t;

/* this is used both for creating a new list (list is null) and adding */
linked_list_t* linked_list_append(linked_list_t* list, void* data) {
    /* create a reference to modify */
    linked_list_t *list_inner = list;
    /* claim some memory and get a pointer to it */
    linked_list_t *new_list = (linked_list_t*)malloc(sizeof(linked_list_t));
    /* fill in our new element (the `->` syntax means 'change the pointed
    object' */
    new_list->data = data;
    new_list->next = 0;

    /* if there is no list (void pointer) return current list */
    if (list == 0) {
        return new_list;
    /* else add this new list to the end of an existing list */
    } else {
        /* get the last element (element where next is void) */
        while (list_inner->next != 0) {
            list_inner = list_inner->next;
        }
        /* make our new list the next element */
        list_inner->next = new_list;
    }
    return list;
}

/* remove an element from the list and return it */
void* linked_list_remove(linked_list_t* list, int idx) {
}

/* get a reference to an element in the list */
void* linked_list_nth(linked_list_t* list, int idx) {
}

```

This example was quite complicated, but most of the details are not important.
What is important is the way pointers can be used to create complex data
structures, like this list that can be any length and hold any type of data.
The data doesn't even need to all be the same type or size, as however big the
data is, the pointers will be the same.

Unfortunately, programming like this is very error-prone. We are relying on the
programmer to correctly interperet the types of the data. If this is done
incorrectly, we can do a whole host of horrible things, including but not
limited to:

 1. crash the program (this is possibly the best outcome we can hope for),
 2. introduce an error into our memory (imagine changing the amount in a bank
    transaction), or
 3. allow a user who can control some of the memory (e.g. by setting a value) 
    to take control of the computer.

When computers were invented, the idea of being able to do anything was new,
and whilst program correctness was important, the theory of what that means was
young. One of the concepts that really nails down some of these errors is the
concept of *types*. Types are a way of demarking different types of data, so
where the processor just sees a byte, we see an integer, or a float, or a
character, or a pointer etc. We may even have our own types not well
represented by these raw types, for example date, or currency (with requires
exactly 2 decimal points, and does not allow floating point error).

These types aren't necessary. They are there to stop us doing things we don't
mean to do. This is a key reason for the existance of programming languages -
as well as making things more concise and easy to reason about, they outright
prevent us from doing lots of things that while possible on raw hardware, are
undesirable. One of the key differences between languages is what they do and
don't allow us to do.

In C, the type system is very permissive, and assumes you know what you're
doing. It allows you to declare that something is one type, when the compiler
thinks it is something else (casting pointers). You can make up a number,
assign it to a pointer and then read it like it was memory (don't do this, it
will crash your computer or worse). Having a permissive type system gives you a
lot of power, but (as the saying goes) also gives you a lot of responsibility
to code correctly.

As languages have evolved over time, and new languages have been created, their
purpose has either been

 1. to lower the barrier of entry by making a language where programs are 
    simpler and easier to maintain, or
 2. restricting what is possible to help the programmer avoid mistakes.

Unfortunately, trying to achieve these goals can lead to some negative
side-effects, including

 1. decreasing performance i.e. increasing running time, and
 2. disallowing useful functionality.

With all languages there are a trade-off between these competing concerns. In
the next chaper I am going to talk about how Rust positions itself (spoiler: it
says the language must be memory-self, and then we'll try and make it as
fast/useable as possible).

