```python
struct.unpack("I", "ABCD")[0] # I specifies unsigned int, and we use [0] to get the last element.
```

It’s important to pay attention to the __endianess__. To change the endinannes just put a `>` nexto the `I` like so: `>I`

There also is struct pack, doing the opposite of unpack. 