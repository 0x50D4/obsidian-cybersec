```ad-note
To find these our best bet is to use the **same value technique**
```

Where we try to access the same file with different paths, for example: `/images/photo.jpg`
- `/images/./photo.jpg` this should give you the <mark style="background: #BBFABBA6;">same file</mark>
- `/images/../photo.jpg` should <mark style="background: #FF5582A6;">error</mark>
- `/images/../images/photo.jpg` should give you the <mark style="background: #BBFABBA6;">same file</mark>
- `/images/../IMAGES/photo.jpg` should <mark style="background: #FF5582A6;">error</mark>, but this depends on the file system.

Now if you made sure this works, we can try other paths such as:
`/etc/passwd` on linux. Best is if you do this by:
```linux
images/../../../../../../../../../../../etc/passwd
```

```ad-tip
If you have path traversal on Windows, you will be able to access the file using `test../../../file.txt` even if `test` does **not** exist.

This is **not** the case on Linux, so this can be used to detect OS.
```

```ad-tip
Another case when the above thing could be useful is for example this code: 
```bash
$file = "/var/files/example_".$_GET['id'].".txt";
``````

In the above code block, if it’s a windows pc 🖥️ we can exploit it since `example_` can be interpreted as a non existent folder. 

We could find an exploitable url for example, in an image path.

There might be simple checks you can bypass by keeping some of the original path there, for example like this: 
```url
file.php?file=/var/www/../../../../../pentesterlab.key
```

## Nullbyte bypass

Sometimes the server forces an extension to a file trying to keep you from accessing files with bad extensions. But this can be bypassed in `perl` and older versions of `php` by using a nullbyte `%00`

```ad-attention
PHP has this patched since __5.3.4__
```

