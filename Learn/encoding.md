


A computer cannot store "letters", "numbers", "pictures" or anything else. The only thing it can store and work with are bits. A bit can only have two values: yes or no, true or false, 1 or 0.

To use bits to represent anything at all besides bits, **we need rules**. **We need to convert a sequence of bits into something like letters, numbers and pictures** using encoding. Like this:

```
01100010 01101001 01110100 01110011
b        i        t        s
```

**In this encoding**, 01100010 stands for the letter "b", 01101001 for the letter "i", 01110100 stands for "t" and 01110011 for "s".

A certain sequence of bits stands for a letter and a letter stands for a certain sequence of bits. If you can keep this in your head for 26 letters or are really fast with looking stuff up in a table, you could read bits like a book.

The above encoding scheme happens to be ASCII. A string of 1s and 0s is broken down into parts of eight bit each (a byte for short). The ASCII encoding specifies **a table translating bytes into human readable letters**.