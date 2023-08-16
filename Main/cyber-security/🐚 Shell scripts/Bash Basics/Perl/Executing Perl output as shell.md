```bash
$(perl -e 'print "uname"';)

output: Linux
```

## In between

```bash
una$(perl -e 'print "m";')e

output: Linux
```

## Using tilde keys

```bash
u`perl -e 'print "na";'`me

output: Linux
```

This works the same as [[#in between]]

