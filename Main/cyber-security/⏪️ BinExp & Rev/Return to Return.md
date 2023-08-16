Basically, if the addresses you can jump to are restricted, meaning they are read, and if you jump to anything on the stack the program yells at you, there is a little exploit you can do.

You should try returning to a `ret` instruction on the stack, then put the actual address you wanna go to on the next place on stack, allowing you to jump without being checked.