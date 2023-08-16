File include vulns can be dangerous for several reasons, they can allow an attacker to read local files, and if that wasn’t enough, also sometimes interpret and execute remote or local php files, leading to an __RCE__.

```ad-note
PHP by default turns this off with: `allow_url_include` 
```

This can a lot of times be tested by injecting a a special character (for example a _quote_).

The problem usually occurs in: `require`, `require_once`, `include` or `include_once`.

