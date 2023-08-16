Grep can filter out results from an output.
Examples:

```bash
find /home -name .bashrc -exec grep [PATTERN] {} \;
``` 
This code lists all `.bashrc` files, and greps a pattern in the file text.

To ask grep to finds something and provide the following line too:
```bash
grep -A [numoflines] "pattern"
```

To only include lines starting with the string:
```bash
grep "^pattern"
```

