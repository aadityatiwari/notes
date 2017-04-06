[The NUXI Problem](https://betterexplained.com/articles/understanding-big-and-little-endian-byte-order/#The_NUXI_Problem)
---------
Issues with byte order are sometimes called the NUXI problem: UNIX stored on a big-endian machine can show up as NUXI on a little-endian one.

Suppose we want to store 4 bytes (U, N, I and X) as two shorts: UN and IX. Each letter is a entire byte, like our WXYZ example above. To store the two shorts we would write:
```c
short *s; // pointer to set shorts
s = 0; // point to location 0
*s = UN; // store first short: U * 256 + N (fictional code)
s = 2; // point to next location
*s = IX; // store second short: I * 256 + X
```

This code is not specific to a machine. If we store "UN" on a machine and ask to read it back, it had better be "UN"! I don't care about endian issues, if we store a value on one machine and read it back on the same machine, it must be the same value.

However, if we look at memory one byte at a time (using our char \* trick), the order could vary. On a big endian machine we see:
```
Byte: U N I X
Location: 0 1 2 3
```
Which make sense. U is the biggest byte in "UN" and is stored first. The same goes for IX: I is the biggest, and stored first.

On a little-endian machine we would see:
```
Byte: N U X I
Location: 0 1 2 3
```
And this makes sense also. "N" is the littlest byte in "UN" and is stored first. Again, even though the bytes are stored "backwards" in memory, the little-endian machine *knows* it is little endian, and interprets them correctly when reading the values back. Also, note that we can specify hex numbers such as x = 0x1234 on any machine. Even a little-endian machine knows what you mean when you write 0x1234, and won't force you to swap the values yourself (you specify the hex number to write, and it figures out the details and swaps the bytes in memory, under the covers. Tricky.).

This scenario is called the "NUXI" problem because byte sequence UNIX is interpreted as NUXI on the other type of machine. Again, this is only a problem if you exchange data -- each machine is internally consistent.
