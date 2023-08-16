```ad-important
Stacks grow downwards and heaps grow upwards!
![[Drawing 2023-03-05 09.49.17.excalidraw|300]]
```

Heap is a region of memory that is not managed by the CPU, it's a free floating region of memory.

In C, to allocate memory, we must use `malloc()` or `calloc()` and to free it, we use `free()`
If you don't free memory, you might have a memory leak.

Unlike the stack, the heap does not have size restrictions other than physical.

Heap variables are accessible to any function anywhere in the program.




