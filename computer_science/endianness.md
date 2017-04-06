
[Endianness](https://en.wikipedia.org/wiki/Endianness)
-----------

-   Endianness refers to the sequential order used to numerically interpret a range of **bytes** in computer memory as a larger, composed word value. It also describes the **order of byte transmission** over a digital link.

-   Words may be represented in **big-endian** or **little-endian** format.

-   When storing a word in big-endian format the MS byte, which is the **byte containing the MS bit**, is stored first and the following bytes are stored in decreasing significance order with the LS byte, which is the **byte containing the LS bit**, thus being stored at last place.

-   Little-endian format reverses the order of the sequence and stores LS byte at the first location with the MS byte being stored last. The order of bits within a byte can also have Endianness; however, a byte is typically handled as a numerical value or character symbol and so bit sequence order is obviated.

-   Big-endian is the most common format in data networking; fields in the protocols of the Internet protocol suite, such as IPv4, IPv6, TCP, and UDP, are transmitted in big-endian order. For this reason, **big-endian byte order is also referred to as network byte order**.

-   Little-endian storage is popular for microprocessors, in part due to significant influence on microprocessor designs by Intel Corporation.

-   One hundred twenty-three: 123 (big-endian) and 321 (little-endian). Endianness in computing is similar, but it usually applies to the ordering of bytes, rather than of digits.

    <img src="https://upload.wikimedia.org/wikipedia/commons/5/54/Big-Endian.svg" alt="Big-Endian" width="279" height="249" /><img src="https://upload.wikimedia.org/wikipedia/commons/e/ed/Little-Endian.svg" alt="Little-Endian" width="279" height="249" />

-   Big-endian is akin to left-to-right reading in hexadecimal order. (**big-end first**)

-   Little-endian is akin to right-to-left reading in hexadecimal order. (**little-end first**)

-   Bit numbering is a concept similar to Endianness, but on a level of bits, not bytes.

-   The bit-level analogue of little-endian (LS bit goes first) is used in RS-232, Ethernet, and USB.

-   Issues with byte order are sometimes called the NUXI problem: UNIX stored on a big-endian machine can show up as NUXI on a little-endian one.
