# Why Programming Languages Exist

At its most basic level, a computer is a machine that manipulates electrical
signals. The key technologies are semiconductor electronic components like
transistors and diodes. Because these work at a very small scale, they allowed
the minaturaization of what had been large mechanical or valve-based computing
machines, like the Colossus at Bletchly Park. Smaller components mean you can
fit more of them in an area, and smaller distances between and in components
mean electrical signals can move faster and computations can happen faster.

To run a CPU, a square wave is created with a given frequency. This provides
the power to change state of components inside the chip. The frequency is often
given as a measure of the speed of the CPU, although many other factors impact
on this (cache speed/size, memory speed, layout of the chip etc.).

The chip will read instructions from a list fed in from memory and execute
them. These instructions will cause different parts of the circuitry in the
semiconductor to be used and the output from those circuitry indicates
modifications the memory should make to itself (e.g. increment a number).

Some instructions will cause the processor to *jump* to an arbitary location in
memory rather than read the next instruction. This jump can be conditional on
some state of memory. This functionality is used to implement branching like
if-then-else and goto. Loading memory, arithmetic and branching are essentially
all a computer can do. Luckily this is enough to power everything computers
are used for today.

> Aside: Most of the time a program will contain both instructions and data in
> the same block of memory. The processor has no way of knowing if it should
> read some memory as an instruction or as data. This has to either be checked
> by a human when writing the code, or by a programming language or runtime
> (e.g. the java virtual machine). Any programming errors here might allow a
> user of the system to write any data they like to a location and then have it
> read as an instruction. This is **very bad**. They would typically execute a
> jump into some memory they control and then run their instructions,
> effectively taking control of the machine. A large class of security
> vulnerabilities in modern code take advantage of mistakes of this type. Rust
> aims to provide checks at compile time to make these bugs impossible.

Machine code is not very easy to read. For example, here is a program that
prints "hello world" and exits (rendered in hex, elf64 bin format).

```
00000000: 7f45 4c46 0101 0100 0000 0000 0000 0000  .ELF............
00000010: 0200 0300 0100 0000 8080 0408 3400 0000  ............4...
00000020: a801 0000 0000 0000 3400 2000 0200 2800  ........4. ...(.
00000030: 0600 0500 0100 0000 0000 0000 0080 0408  ................
00000040: 0080 0408 a200 0000 a200 0000 0500 0000  ................
00000050: 0010 0000 0100 0000 a400 0000 a490 0408  ................
00000060: a490 0408 0c00 0000 0c00 0000 0600 0000  ................
00000070: 0010 0000 0000 0000 0000 0000 0000 0000  ................
00000080: ba0c 0000 00b9 a490 0408 bb01 0000 00b8  ................
00000090: 0400 0000 cd80 bb00 0000 00b8 0100 0000  ................
000000a0: cd80 0000 4865 6c6c 6f20 576f 726c 640a  ....Hello World.
000000b0: 0000 0000 0000 0000 0000 0000 0000 0000  ................
000000c0: 0000 0000 8080 0408 0000 0000 0300 0100  ................
000000d0: 0000 0000 a490 0408 0000 0000 0300 0200  ................
000000e0: 0100 0000 0000 0000 0000 0000 0400 f1ff  ................
000000f0: 0c00 0000 a490 0408 0000 0000 0000 0200  ................
00000100: 1000 0000 0c00 0000 0000 0000 0000 f1ff  ................
00000110: 1400 0000 b090 0408 0000 0000 1000 0200  ................
00000120: 2000 0000 8080 0408 0000 0000 1000 0100   ...............
00000130: 2500 0000 b090 0408 0000 0000 1000 0200  %...............
00000140: 2c00 0000 b090 0408 0000 0000 1000 0200  ,...............
00000150: 0073 696d 706c 652e 6173 6d00 6d73 6700  .simple.asm.msg.
00000160: 6c65 6e00 5f5f 6273 735f 7374 6172 7400  len.__bss_start.
00000170: 6d61 696e 005f 6564 6174 6100 5f65 6e64  main._edata._end
00000180: 0000 2e73 796d 7461 6200 2e73 7472 7461  ...symtab..strta
00000190: 6200 2e73 6873 7472 7461 6200 2e74 6578  b..shstrtab..tex
000001a0: 7400 2e64 6174 6100 0000 0000 0000 0000  t..data.........
000001b0: 0000 0000 0000 0000 0000 0000 0000 0000  ................
000001c0: 0000 0000 0000 0000 0000 0000 0000 0000  ................
000001d0: 1b00 0000 0100 0000 0600 0000 8080 0408  ................
000001e0: 8000 0000 2200 0000 0000 0000 0000 0000  ...."...........
000001f0: 1000 0000 0000 0000 2100 0000 0100 0000  ........!.......
00000200: 0300 0000 a490 0408 a400 0000 0c00 0000  ................
00000210: 0000 0000 0000 0000 0400 0000 0000 0000  ................
00000220: 0100 0000 0200 0000 0000 0000 0000 0000  ................
00000230: b000 0000 a000 0000 0400 0000 0600 0000  ................
00000240: 0400 0000 1000 0000 0900 0000 0300 0000  ................
00000250: 0000 0000 0000 0000 5001 0000 3100 0000  ........P...1...
00000260: 0000 0000 0000 0000 0100 0000 0000 0000  ................
00000270: 1100 0000 0300 0000 0000 0000 0000 0000  ................
00000280: 8101 0000 2700 0000 0000 0000 0000 0000  ....'...........
00000290: 0100 0000 0000 0000                      ........
```

For this reason, instructions are written in a language where different symbols
correspond to different instructions to the machine. For example, the command

```asm
mov eax,10
```

loads the value `10` into the processor, ready to do some kind of operation.
This would then get turned into some unreadable values in a step called
*assembly*, so the machine can understand what to do. Note that although the
commands are readable by a human, there is a direct correspondence between them
and the commands the machine receives, so we are really still writing in the
machine's language. For example, if we wanted to multiply a number by another,
we need to load the numbers into the processor, perform the multiplication, and
move the result back into memory: something like

```asm
mov ebx,10
mul ebx,8 ; ebx now contains 80
mov [0xa2f43], ebx ; move the result somewhere to store it
```

What we really want to write is `10 * 8` and have the computer worry about how
to turn this expression into some instructions it can run. To make this
possible the concept of a `compiler` was invented. A compiler is different from
an assembler, as instead of just changing the respresentation of some
instructions, it actually re-writes the instructions into a new equivalent
form.

Here is what our conceptual compiler needs to do:
 1. Recognise that `10`, `*`, and `8` are separate logical tokens
 2. Work out that `10` then `*` then `8` means we need to do a multiplication
    where the parameters are `10` and `8`. Internally it is helpful to
    think of this as a single `mul` block where the parameters are `10` and
    `8`.
 3. Write some machine instructions to perform the requested operation,
    something like those above.

The language we have just created is a lot like a human language. It has words
(like `10` and `*`), and sentences like `10 * 8`. The letters are latin letters
we use for English. There are some key differences between computer languages
and human languages: in general human languages have a huge number of words,
and the same word or phrase can mean many different things, often dependent on
what is said elsewhere in the text. These 2 traits are considered bad in
programming as they make it harder to reason about what a program is doing,
both for the user and the computer.

In both real languages and computer languages we sometimes need names for
things. In English, we would use a proper noun like `London` or `James`,
whereas in programming languages the rule is usually *any number of letters and
numbers where the first character is a letter*, like `x` or `c3po`.

Compiler design and implementation is a huge topic covering many different
areas, but the key steps are as above: parse a stream of text into a
structure representing its meaning, then write it out to a stream in a
different language (usually machine language).

Choices about how a language is made have huge impacts on the way a language is
used, and as such a lot of time will go into working out the most ergonomic and
productive ways to express the functionality you want, whilst making it as hard
as possible to write incorrect code.
