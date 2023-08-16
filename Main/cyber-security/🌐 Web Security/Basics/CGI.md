## How does it work?

When you call a CGI, the web server runs a new process to run the CGI. This will start a Bash Process and run the __CGI script__.

Apache uses headers to pass information to the CGI script. If you have a header named `Blah` in your request, it will make an environment variable called `HTTP_BLAH`

```ad-note
You can quickly test this by replacing the call to `uptime` with `env` in the CGI. Then if you call the script with an arbitrary header, it should appear on the page.
```

