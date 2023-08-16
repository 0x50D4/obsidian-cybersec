---
tags: ldap
---

An LDAP server ofc doesn’t authenticate you if your credentials are incorrect, but sometimes it authorizes a __NULL Bind__.

If null values are sent, the server with proceed to bind the connection.

For this reason a php code might think that the credentials are correct.

```ad-important
It is very important to test this even on non-ldap based servers.
```

```ad-important
You need to remove both queries compleatly, `username=&password=` is not okay in the url, remove it.
```

