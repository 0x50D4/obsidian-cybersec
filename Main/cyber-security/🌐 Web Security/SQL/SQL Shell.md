This is a guide on how to exploit an SQL injection to gain shell on a PHP website.

There are 3 phases:
- Fingerprinting: Gathering data to see what kind of website this is
- Detection and exploitation: Learn how SQL injections work
- Access to admin pages and code execution


# Fingerprinting
A lot of data can be retrieved from the response headers. So look out for that.

We can also __fuzz__ directories with [[wfuzz]] or [[gobuster]]:
```bash
$ python wfuzz.py -c -z file,wordlist/general/big.txt --hc 404 http://vulnerable/FUZZ
```

# Detection and exploitation of SQL injections

Here is an example query an admin might use: 

```sql
SELECT column1, column2, column3 FROM table1 WHERE column4='string1'  AND column5=integer1 AND column6=integer2;
```

