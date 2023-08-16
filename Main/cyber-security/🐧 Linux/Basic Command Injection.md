---
exploit: "command-injection"
---
Let’s say we have a basic website where we can enter an IP, and that is ran with #python(🐍) for example using `popen` and `.read()` to input the data. What could the vuln be here? 🤔

```ad-important
It does not actually work with popen, since that can only run one command and arguments are passed separately. But it would work with os.system()
```

Well, we can use __command injection__. So, let's say this is how a url looks like:
```url
http://example.url/?ip=127.0.0.1
```

We could use several things:
- `command1 && command2` where the __second__ runs if `command1` succeeds
- `command1 || command2` where the __second__ runs if `command1` fails
- `command1 || command2` where the __second__ runs after the __first__ one.
- `command1 | command2` where the output of `command1` is _piped_ into `command2`

So we can just do:
```url
http://example.url/?ip=127.0.0.1; cat /etc/passwd
```

this is how it would look on the server side:
```bash
ping 127.0.0.1 ; cat /etc/passwd
```

If characters like this are blocked, there is still a chance that the developers forgot that you can run commands with \`, since that evaluates the result and uses that as an argument. You can also use `$([command])` for this.

