# A way to get data out of a file

```
CREATE TABLE demo(t text);
COPY demo from '[FILENAME]'
SELECT * from demo;
```

